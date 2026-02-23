
#include <16F877A.h>
#fuses HS, NOWDT, NOLVP, NOPUT, NOPROTECT
#use delay(clock=20000000)

void blink_leds(char port, unsigned int8 mask) {
    for (int i = 0; i < 2; i++) {
        if (port == 'B') {
            output_b(output_b() & ~mask); // Aktif LOW › LED yanar
            delay_ms(250);
            output_b(output_b() | mask);  // LED söner
            delay_ms(250);
        } else if (port == 'D') {
            output_d(output_d() & ~mask); // Aktif LOW › LED yanar
            delay_ms(250);
            output_d(output_d() | mask);  // LED söner
            delay_ms(250);
        }
    }
}

void main() {
    // RB0–RB3 ve RD4–RD7 giriş (buton), RB4–RB7 ve RD0–RD3 çıkış (LED transistör kontrolü)
    set_tris_b(0x0F); // RB0–RB3 giriş, RB4–RB7 çıkış
    set_tris_d(0xF0); // RD4–RD7 giriş, RD0–RD3 çıkış

    output_b(0xF0); // LED çıkışları başlangıçta HIGH › LED kapalı
    output_d(0x0F); // LED çıkışları başlangıçta HIGH › LED kapalı

    while (TRUE) {
        // RB0–RB3 butonları › RD0–RD3 LED'leri
        if (!input(PIN_B0)) blink_leds('D', 0x01); // RD0
        if (!input(PIN_B1)) blink_leds('D', 0x02); // RD1
        if (!input(PIN_B2)) blink_leds('D', 0x04); // RD2
        if (!input(PIN_B3)) blink_leds('D', 0x08); // RD3

        // RD4–RD7 butonları › RB4–RB7 LED'leri
        if (!input(PIN_D4)) blink_leds('B', 0x10); // RB4
        if (!input(PIN_D5)) blink_leds('B', 0x20); // RB5
        if (!input(PIN_D6)) blink_leds('B', 0x40); // RB6
        if (!input(PIN_D7)) blink_leds('B', 0x80); // RB7
    }
}
