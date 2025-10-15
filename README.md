# parse-protobuf-demo

The goal is to demo the parsing of a bid request from Google Authorized Buyers with the most recent version of the proto file available when this project was created, as well as the most recent protobuf and protobuf-maven-plugin version available.

The issue is that some fields are not exposed in the generated classes, especially fields of the user object, under field 1007 which seems to be the UserExt object.
Our interest is to gain access to the EIDs stored in the UserExt object, in this example the _pubcid.org_.

```
user {
  consent: "CQZVOgAQZVOgAAGABCENB-FsAP_gAEPgAAqIKbMBbGJMSSFgcTp1YOsAaYgewVACqsAgBgKBAQABiBICtIQUkGAAIAhAhiAAAAIACEBAAABlAABAAAAAIAAAACAMCAAAABAAICAAAAABQgAACEAJAQAAAAAQxEBEAAEAgQIE6JpUwAxAAAAACAAAAAAAAACAAAAIAAAAAAAAAQAAAAgAAAAAAAIAAAAAABAAEAAAAAEAAAAAAAAAAAAAAAAAAAAAAAAAEDIYCQACgAHgAkABUAEcARwA7gB9gEAAIQARwA_4CPQExALEAWoAxYBkIBISA5ABQAFQAOAAggBkAGoAPAAmABVADeAH4AQgAjgBWgDDAGUAOcAdwA-wB-gEAANEAbQBR4C2AF5gMNgZGBkgDVwHJjoEMAFAAVAA4ACCAGQAagA8ACYAFWALgAugBiADeAH4AUYArQBhgDKAHOAO4AfYA_QCLAEdANoAmQBR4C2AF5gL6AYaAx6BkYGSANXAcmBHYcABACgQgFgAUABqAFUALgAYgA3gB-AHOAO4A2gDIyUA8ACgAHAAeABMACqAFwAMQAjgBRgCtAUeApoBbAC84GRgZIUgNQAUABUADgAIIAZABoADwAJgAVQAxAB-AFGAK0AZQA5wB-gEWAI6AbQBTQC2AF5gL6AYbAyMDJAHJgQ5AjsVAAgAKKAAwAoABbAI4WgBgA1AHcApoAAA.f_wAAAAAAAAA"
  1007: {
    1: {
      2: "\356\003\206\026Y"
      3: "1~89.494.2822"
    }
    2: "CQZVOgAQZVOgAAGABCENB-FsAP_gAEPgAAqIKbMBbGJMSSFgcTp1YOsAaYgewVACqsAgBgKBAQABiBICtIQUkGAAIAhAhiAAAAIACEBAAABlAABAAAAAIAAAACAMCAAAABAAICAAAAABQgAACEAJAQAAAAAQxEBEAAEAgQIE6JpUwAxAAAAACAAAAAAAAACAAAAIAAAAAAAAAQAAAAgAAAAAAAIAAAAAABAAEAAAAAEAAAAAAAAAAAAAAAAAAAAAAAAAEDIYCQACgAHgAkABUAEcARwA7gB9gEAAIQARwA_4CPQExALEAWoAxYBkIBISA5ABQAFQAOAAggBkAGoAPAAmABVADeAH4AQgAjgBWgDDAGUAOcAdwA-wB-gEAANEAbQBR4C2AF5gMNgZGBkgDVwHJjoEMAFAAVAA4ACCAGQAagA8ACYAFWALgAugBiADeAH4AUYArQBhgDKAHOAO4AfYA_QCLAEdANoAmQBR4C2AF5gL6AYaAx6BkYGSANXAcmBHYcABACgQgFgAUABqAFUALgAYgA3gB-AHOAO4A2gDIyUA8ACgAHAAeABMACqAFwAMQAjgBRgCtAUeApoBbAC84GRgZIUgNQAUABUADgAIIAZABoADwAJgAVQAxAB-AFGAK0AZQA5wB-gEWAI6AbQBTQC2AF5gL6AYbAyMDJAHJgQ5AjsVAAgAKKAAwAoABbAI4WgBgA1AHcApoAAA.f_wAAAAAAAAA"
    5: {
      1: "pubcid.org"
      2: {
        1: "5ac4a28e-858e-4bab-92da-490567f132bc"
      }
    }
  }
}
```

Full execution result:
```
id: "rdEcuWmxFRbGbX2llZ693Q"
imp {
  id: "1"
  banner {
    w: 300
    h: 600
    pos: BELOW_THE_FOLD
    battr: VIDEO_IN_BANNER_AUTO_PLAY
    battr: VIDEO_IN_BANNER_USER_INITIATED
    wmax: 300
    hmax: 600
    wmin: 300
    hmin: 250
    format {
      w: 300
      h: 600
    }
    1014: {
      2: {
        1: 300
        2: 300
        3: 250
        4: 600
      }
    }
  }
  displaymanager: "GOOGLE"
  tagid: "1187298996"
  bidfloor: 0.32
  bidfloorcur: "EUR"
  pmp {
    private_auction: true
    deals {
      id: "1720724"
      bidfloor: 3.5
      bidfloorcur: "EUR"
      at: FIXED_PRICE
      1010: {
        1: 1
        3: 1
        5: {
          1: "\001"
        }
        6: "\236\374\217\220\344\003\355\211\317\341\310\003"
      }
    }
  }
  secure: true
  exp: 3600
  metric {
    type: "click_through_rate"
    value: 8.70223215315491E-4
    vendor: "EXCHANGE"
  }
  metric {
    type: "viewability"
    value: 0.09
    vendor: "EXCHANGE"
  }
  1009: {
    1: 129956576798
    1: 122611287277
    2: 0xad6cf3f55452bdcd
    2: 0x21c7d7c223748094
    2: 0x30e0fc5c4b5b3a56
    3: "\000"
    8: 3
    9: {
      2: "5ac4a28e-858e-4bab-92da-490567f132bc"
    }
    13: 32
    13: 33
    15: {
      1: 2
      2: 1
      3: 1
    }
    18: 0
    66: 0
  }
}
site {
  page: "https://www.lepoint.fr/politique/ca-peut-tanguer-le-ps-a-l-heure-du-choix-face-a-lecornu-ii-13-10-2025-2600831_20.php"
  publisher {
    id: "pub-8140346826098443"
    1002: {
      1: "FR"
    }
  }
  content {
    livestream: false
    language: "fr"
  }
  1010: {
    1: 0
    2: 2
    4: {
    }
    6: 2
  }
}
device {
  ua: "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/140.0.0.0 Safari/537.36"
  ip: "217.195.28.0"
  geo {
    lat: 48.83
    lon: 2.24
    country: "FRA"
    region: "FR-J"
    city: "Boulogne-Billancourt"
    zip: "92100"
    type: IP
    utcoffset: 120
    accuracy: 1396
    1004: {
      1: 9056493
    }
  }
  os: "Windows"
  devicetype: PERSONAL_COMPUTER
  pxratio: 1.0
  sua {
    browsers {
      brand: "Chromium"
      version: "140"
      version: "0"
      version: "7339"
      version: "208"
    }
    browsers {
      brand: "Not=A?Brand"
      version: "24"
      version: "0"
      version: "0"
      version: "0"
    }
    browsers {
      brand: "Google Chrome"
      version: "140"
      version: "0"
      version: "7339"
      version: "208"
    }
    platform {
      brand: "Windows"
      version: "15"
      version: "0"
      version: "0"
    }
    mobile: false
    architecture: "x86"
    bitness: "64"
    source: CLIENT_HINTS_HIGH_ENTROPY
  }
  1066: {
  }
}
user {
  consent: "CQZVOgAQZVOgAAGABCENB-FsAP_gAEPgAAqIKbMBbGJMSSFgcTp1YOsAaYgewVACqsAgBgKBAQABiBICtIQUkGAAIAhAhiAAAAIACEBAAABlAABAAAAAIAAAACAMCAAAABAAICAAAAABQgAACEAJAQAAAAAQxEBEAAEAgQIE6JpUwAxAAAAACAAAAAAAAACAAAAIAAAAAAAAAQAAAAgAAAAAAAIAAAAAABAAEAAAAAEAAAAAAAAAAAAAAAAAAAAAAAAAEDIYCQACgAHgAkABUAEcARwA7gB9gEAAIQARwA_4CPQExALEAWoAxYBkIBISA5ABQAFQAOAAggBkAGoAPAAmABVADeAH4AQgAjgBWgDDAGUAOcAdwA-wB-gEAANEAbQBR4C2AF5gMNgZGBkgDVwHJjoEMAFAAVAA4ACCAGQAagA8ACYAFWALgAugBiADeAH4AUYArQBhgDKAHOAO4AfYA_QCLAEdANoAmQBR4C2AF5gL6AYaAx6BkYGSANXAcmBHYcABACgQgFgAUABqAFUALgAYgA3gB-AHOAO4A2gDIyUA8ACgAHAAeABMACqAFwAMQAjgBRgCtAUeApoBbAC84GRgZIUgNQAUABUADgAIIAZABoADwAJgAVQAxAB-AFGAK0AZQA5wB-gEWAI6AbQBTQC2AF5gL6AYbAyMDJAHJgQ5AjsVAAgAKKAAwAoABbAI4WgBgA1AHcApoAAA.f_wAAAAAAAAA"
  1007: {
    1: {
      2: "\356\003\206\026Y"
      3: "1~89.494.2822"
    }
    2: "CQZVOgAQZVOgAAGABCENB-FsAP_gAEPgAAqIKbMBbGJMSSFgcTp1YOsAaYgewVACqsAgBgKBAQABiBICtIQUkGAAIAhAhiAAAAIACEBAAABlAABAAAAAIAAAACAMCAAAABAAICAAAAABQgAACEAJAQAAAAAQxEBEAAEAgQIE6JpUwAxAAAAACAAAAAAAAACAAAAIAAAAAAAAAQAAAAgAAAAAAAIAAAAAABAAEAAAAAEAAAAAAAAAAAAAAAAAAAAAAAAAEDIYCQACgAHgAkABUAEcARwA7gB9gEAAIQARwA_4CPQExALEAWoAxYBkIBISA5ABQAFQAOAAggBkAGoAPAAmABVADeAH4AQgAjgBWgDDAGUAOcAdwA-wB-gEAANEAbQBR4C2AF5gMNgZGBkgDVwHJjoEMAFAAVAA4ACCAGQAagA8ACYAFWALgAugBiADeAH4AUYArQBhgDKAHOAO4AfYA_QCLAEdANoAmQBR4C2AF5gL6AYaAx6BkYGSANXAcmBHYcABACgQgFgAUABqAFUALgAYgA3gB-AHOAO4A2gDIyUA8ACgAHAAeABMACqAFwAMQAjgBRgCtAUeApoBbAC84GRgZIUgNQAUABUADgAIIAZABoADwAJgAVQAxAB-AFGAK0AZQA5wB-gEWAI6AbQBTQC2AF5gL6AYbAyMDJAHJgQ5AjsVAAgAKKAAwAoABbAI4WgBgA1AHcApoAAA.f_wAAAAAAAAA"
    5: {
      1: "pubcid.org"
      2: {
        1: "5ac4a28e-858e-4bab-92da-490567f132bc"
      }
    }
  }
}
at: FIRST_PRICE
tmax: 350
cur: "EUR"
cur: "USD"
bcat: "IAB11-4"
bcat: "IAB13-2"
bcat: "IAB14-1"
bcat: "IAB15-1"
bcat: "IAB19-30"
bcat: "IAB23"
bcat: "IAB23-1"
bcat: "IAB23-10"
bcat: "IAB23-2"
bcat: "IAB23-3"
bcat: "IAB23-4"
bcat: "IAB23-5"
bcat: "IAB23-6"
bcat: "IAB23-7"
bcat: "IAB23-8"
bcat: "IAB23-9"
bcat: "IAB7-39"
bcat: "IAB7-44"
regs {
  gpp: ""
  1001: {
    1: 1
  }
}
source {
  schain {
    complete: true
    nodes {
      asi: "google.com"
      sid: "pub-8140346826098443"
      hp: true
    }
    ver: "1.0"
  }
  1059: {
  }
}
cattax: IAB_CONTENT_1_0
1018: {
  2: "AD8Fdm41b4UkMRQP8PGubvXWrrbC6Fo8hKF3p3mPqkyoA5Qobw3OxdH--nPaCD4Syx9Vueq3Nw"
  4: 1
  5: {
    6: "\003"
    7: 0
  }
}

==================================================
EIDs count: 0

```
