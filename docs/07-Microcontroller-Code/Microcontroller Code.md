---
title: Microcontroller Code
---

#include "mcc_generated_files/system/system.h"
#include "mcc_generated_files/i2c_host/i2c1.h"
#include "mcc_generated_files/uart/uart1.h"
#include <stdio.h>
#include <stdint.h>
#include <stdbool.h>

#define TC74_ADDR   0x4C

#define RING_SIZE   128
volatile uint8_t ring_buf[RING_SIZE];
volatile uint8_t ring_head = 0;
volatile uint8_t ring_tail = 0;

static void uprint(const char *s) {
    while (*s) {
        while (!UART1_IsTxReady());
        UART1_Write((uint8_t)*s++);
    }
}

static bool i2c_wait(void) {
    uint16_t t = 10000;
    while (I2C1_IsBusy() && --t)
        I2C1_Tasks();
    return (t != 0);
}

static void i2c_recover(void) {
    if (I2C1STAT0bits.BFRE)
        return;

    I2C1CON0bits.EN = 0;
    __delay_ms(1);

    I2C1STAT1 = 0x00;
    I2C1STAT1bits.CLRBF = 1;
    I2C1PIR  = 0x00;
    I2C1ERR  = 0x00;
    I2C1CNT  = 0x00;

    I2C1CON0bits.EN = 1;
    __delay_us(10);
    I2C1PIRbits.SCIF = 0;
    I2C1PIRbits.PCIF = 0;
}

/* Write register pointer, then Read data, then recover bus */
static bool tc74_read(int8_t *temp) {
    uint8_t reg = 0x00, raw = 0xFF;

    if (!I2C1_Write(TC74_ADDR, &reg, 1))
        return false;
    if (!i2c_wait() || I2C1_ErrorGet() != I2C_ERROR_NONE)
        return false;

    if (!I2C1_Read(TC74_ADDR, &raw, 1))
        return false;
    i2c_wait();
    i2c_recover();

    *temp = (int8_t)raw;
    return (raw != 0xFF);
}

int main(void) {
    char msg[48];

    SYSTEM_Initialize();
    __delay_ms(500);

    PIE4bits.U1RXIE = 1;
    INTCON0bits.GIE = 1;

    uprint("\r\n=== Isaiah Env Sensor ===\r\n");

    uprint("Running.\r\n");

    while (1) {
        int8_t t;
        if (tc74_read(&t)) {
            sprintf(msg, "temp=%d\r\n", t);
            uprint(msg);
        } else {
            uprint("fail\r\n");
            i2c_recover();
        }                    
        __delay_ms(1000);
        if (IO_RB2_GetValue()) {                
        uprint("Reset activated\r\n");
        for (uint8_t i = 0; i < 3; i++) {  
            IO_RB0_SetHigh();
            __delay_ms(250);
            IO_RB0_SetLow();
            __delay_ms(250);
        }
        asm("RESET");   
    }
    }
}
