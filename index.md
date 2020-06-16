# 無線LED表示

## シーケンス図

```sequence
note left of SubGHz(AI1):　被害者検出
SubGHz(AI1)->SubGHz(LED表示): Detect notification
note right of SubGHz(LED表示):　LED表示ON
note right of SubGHz(AI2):　被害者検出
SubGHz(AI2)->SubGHz(LED表示): Detect notification
note right of SubGHz(LED表示):　ロータリーSW変更\nフラグ更新
note right of SubGHz(LED表示):　LED表示OFF
SubGHz(LED表示)->SubGHz(AI1): Get RSSI
SubGHz(AI1)-->SubGHz(LED表示): ACK
SubGHz(LED表示)->SubGHz(AI2): Get RSSI
SubGHz(AI2)-->SubGHz(LED表示): ACK
note right of SubGHz(LED表示):　電波状況を2段階以上で表示
```

## パケットフレーム

|   フレームタイプ    |                             説明                             |
| :-----------------: | :----------------------------------------------------------: |
| Detect notification | PANID：ABCD以外、宛先アドレス： LED表示SubGHzのアドレス(※1)、<br/>ACK：なし、100ms間隔で3回送信する。 |
|      Get RSSI       | PANID：ABCD以外、宛先アドレス： AI SubGHzのアドレス(※1)、<br/>ACK：なし、各AIに100ms間隔で3回づつ送信する。 |

(※1)SubGHzアドレスは無線モジュールの缶シールド面に記載された個体識別コード(MACアドレス)の下位4桁



## フレーム構造

| Octets:1                                                     | 1                                                         |
| ------------------------------------------------------------ | --------------------------------------------------------- |
| frame type                                                   | BT param                                                  |
| Detect notification: 0b1<br />Get RSSI: 0b2<br />reserved: 0b3〜0b8 | LED表示ON: 0b1<br />RSSI取得: 0b2<br />reserved: 0b3〜0b8 |

レイヤ2以下はIEEE802.15.4準拠