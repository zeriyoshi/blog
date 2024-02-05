---
title: Raspberry Pi Pico でピコちゃんを再生する
description: ピコレッタ
slug: 20240205_pico
date: 2024-02-05 00:00:00+0900
categories:
    - JCDK
    - diary
weight: 1
---

[邪フェス2](https://jashinchan.com/news/10179) で邪神ちゃんしっぽマクロパッド (テンキー) を出すために久々に電子工作を再開しました。

前は自作キーボードと言えば Atmel (現: [Microchip](https://www.microchip.co.jp/)) の ATMega32U4 を用いたものが一般的でしたが、最近は Raspberry Pi 財団の開発した RP2040 が安くて (動作が) 早いということで主流になりつつあるようです。

## PICO-BOOT (Raspberry Pi Pico 互換機) 購入

というわけで [Raspberry Pi Pico](https://www.raspberrypi.com/products/raspberry-pi-pico/) を購入しに秋葉原に向かったのですが、 **外部接続端子が USB microB** だということを完全に忘れていました。いくら 700 円程度と安いとはいえ、今どき壊れやすくてケーブル調達のダルい microUSB を使いたくない...

そんなことを思いながら[遊舎工房](https://yushakobo.jp/)の棚を眺めていたら、 USB C 端子にした `PICO-BOOT` という互換機が[売られていました。](https://shop.yushakobo.jp/products/7532)お値段 1100 円。それでも ATMega32U4 の Pro Micro 互換機より安い...

[QMK](https://qmk.fm/) (オープンソースのキーボードファームウェア) 側が対応しているのも確認済みだし、実際に頒布するやつでも使おう、ということで 10 個 + 1 個 (試作用) をまとめ買いしちゃいました。お値段 12100 円。

あとせっかくだから X (旧: Twitter) でバズりやすい、というかキャッチーな感じを演出できるよう音を出して遊ぶための小道具を購入しました。

## DFPlayer Mini で microSDHC カードから音楽を鳴らす

まずは [DFPlayer Mini](https://wiki.dfrobot.com/DFPlayer_Mini_SKU_DFR0299) を使って microSDHC カードにある音楽を再生してみることにしました。

基板に [MicroPython](https://micropython.org/) ランタイムを書き込み、簡単なコードを書いてブレッドボードに配線...動きました

{{< gist zeriyoshi 8214a4d846ef42861f16f2383a1e4127 >}}

{{< tweet user="zeriyoshi_s" id="1754151048883863925" >}}

まあ動いたんだけど、これって結局 DFPlayer Mini に再生命令送ってるだけだし、うーん...

## PWM を用いて直接スピーカーから再生

単体で再生したい！ということでいろいろ調べてみると、どうやら [Adafruit](https://www.adafruit.com/) の [CircuitPython](https://circuitpython.org/) を使えば PWM を用いて簡単に音楽が再生できそうだということに。

`BOOTSEL` ボタンを押しながら USB 接続して CircuitPython の `uf2` ファイルをコピーして書き込み、コードを書いて...動いた！

{{< gist zeriyoshi 3ec0d518e54cb81b04e5b300453b6ee3 >}}

{{< tweet user="zeriyoshi_s" id="1754480535089582247" >}}

PWM だけでやっててアンプがかまさってないから音量は小さめだけど、十分満足な感じ。

それはそうとこの PICO-BOOT とやら、 16MB (bit じゃなく byte) もの Flash ROM がついてて何なら単体で音楽プレーヤーにできそう... すごい時代ですね
