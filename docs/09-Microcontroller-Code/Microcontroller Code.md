---
title: Microcontroller Code
---

 * main.c  --  Team 305  |  Isaiah  |  Temperature Sensor Node  (0x49)
 * =====================================================================
 * MCU  : PIC18F57Q43
 * IDE  : MPLAB X + MCC Melody
 * Bus  : UART1  9600 baud  daisy-chain
 * Sensor: TC74Ax  I2C temperature sensor

#include "mcc_generated_files/system/system.h"
#include "mcc_generated_files/i2c_host/i2c1.h"
#include "mcc_generated_files/uart/uart1.h"
#include "team_api.h"
#include <stdio.h>
#include <stdint.h>
#include <stdbool.h>
#include <string.h>

#define RING_SIZE 128
volatile uint8_t ring_buf[RING_SIZE];
volatile uint8_t ring_head = 0;
volatile uint8_t ring_tail = 0;

#define MY_ID           NODE_ISAIAH
#define TC74_ADDR       0x4C
#define LED_BLINK_MS    80u
#define LOOP_DELAY_MS   200u

static void led_blink(void) {

    IO_RB0_SetHigh();
    __delay_ms(LED_BLINK_MS);
    IO_RB0_SetLow();
}

static void uprint(const char *s) {

    while (*s) {
        while (!UART1_IsTxReady());
        UART1_Write((uint8_t)*s++);
    }
}

static void print_hex(const uint8_t *buf, uint8_t len) {

    char tmp[6];
    for (uint8_t i = 0; i < len; i++) {
        sprintf(tmp, "%02X", buf[i]);
        uprint(tmp);
        if (i + 1 < len) uprint(" ");
    }
}

static bool i2c_wait(void) {

    uint16_t t = 10000;
    while (I2C1_IsBusy() && --t) I2C1_Tasks();
    return (t != 0);
}

static void i2c_recover(void) {

    if (I2C1STAT0bits.BFRE) return;
    I2C1CON0bits.EN = 0;  __delay_ms(1);
    I2C1STAT1 = 0x00;  I2C1STAT1bits.CLRBF = 1;
    I2C1PIR = 0x00;  I2C1ERR = 0x00;  I2C1CNT = 0x00;
    I2C1CON0bits.EN = 1;  __delay_us(10);
    I2C1PIRbits.SCIF = 0;  I2C1PIRbits.PCIF = 0;
}
static bool tc74_read(int8_t *out_c) {

    uint8_t reg = 0x00, raw = 0xFF;
    if (!I2C1_Write(TC74_ADDR, &reg, 1)) return false;
    if (!i2c_wait() || I2C1_ErrorGet() != I2C_ERROR_NONE) return false;
    if (!I2C1_Read(TC74_ADDR, &raw, 1)) return false;
    i2c_wait();
    i2c_recover();
    *out_c = (int8_t)raw;
    return (raw != 0xFF);
}

int main(void) {

    SYSTEM_Initialize();
    __delay_ms(500);
    IO_RB0_SetLow();
    
    PIE4bits.U1RXIE = 1;
    INTCON0bits.GIE = 1;
    
    uprint("\r\n=== Team 305  |  Isaiah Temp Sensor (0x49) ===\r\n");
    while (1) {
        process_incoming();
        int8_t t;
        if (!tc74_read(&t)) {
            uprint("[WARN] I2C fail - recovering\r\n");
            i2c_recover();
            
        }
        __delay_ms(LOOP_DELAY_MS);
        if (IO_RB2_GetValue()) {
            asm("RESET");
        }
    }
}
