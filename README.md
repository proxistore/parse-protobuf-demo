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

Another bid request sample is parsed to show the content when the field BidRequest.user.eids is populated. That second sample is very interesting as it contains BidRequest.user.eids provided by pub-7418191728211626, as well as BidRequest.user.ext.eids, provided by first-id, and it's exactly the provider that we want to get.

```
user {
  data {
    id: "0"
    segment {
      id: "368"
    }
    1000: {
      1: 4
    }
  }
  eids {
    source: "pub-7418191728211626"
    uids {
      id: "MTh0c2c3NzE4dHNnNzcxOHRzZzc3MTh0"
      1001: {
        1: "ppuid"
      }
    }
  }
  1007: {
    5: {
      1: "first-id.fr"
      2: {
        1: "2ec51a54a56b4d9eb3fb8716cb6d528d"
      }
    }
  }
}
```

Full execution result:
```
==================================================
Protobuf: ChZyZEVjdVdteEZSYkdiWDJsbFo2OTNREs4CCgExEjEIrAIQ2AQgAzICBgdYrAJg2ARorAJw+gF6BgisAhDYBLI/DhIMCKwCEKwCGPoBINgEIgZHT09HTEU6CjExODcyOTg5OTZBexSuR+F61D9KA0VVUlo3CAESMwoHMTcyMDcyNBEAAAAAAAAMQBoDRVVSMAOSPxcIARgBKgMKAQEyDJ78j5DkA+2Jz+HIA2ABcJAcigEnChJjbGlja190aHJvdWdoX3JhdGURAAAAIPaDTD8aCEVYQ0hBTkdFigEgCgt2aWV3YWJpbGl0eREK16NwPQq3PxoIRVhDSEFOR0WKP2gInvyPkOQDCO2Jz+HIAxHNvVJU9fNsrRGUgHQjwtfHIRFWOltLXPzgMBoBAEADSiYSJDVhYzRhMjhlLTg1OGUtNGJhYi05MmRhLTQ5MDU2N2YxMzJiY2ggaCF6BggCEAEYAZABAJAEABqqATp1aHR0cHM6Ly93d3cubGVwb2ludC5mci9wb2xpdGlxdWUvY2EtcGV1dC10YW5ndWVyLWxlLXBzLWEtbC1oZXVyZS1kdS1jaG9peC1mYWNlLWEtbGVjb3JudS1paS0xMy0xMC0yMDI1LTI2MDA4MzFfMjAucGhwWh0KFHB1Yi04MTQwMzQ2ODI2MDk4NDQz0j4ECgJGUmIHaACaAQJmcpI/CAgAEAIiADACKugCEm9Nb3ppbGxhLzUuMCAoV2luZG93cyBOVCAxMC4wOyBXaW42NDsgeDY0KSBBcHBsZVdlYktpdC81MzcuMzYgKEtIVE1MLCBsaWtlIEdlY2tvKSBDaHJvbWUvMTQwLjAuMC4wIFNhZmFyaS81MzcuMzYaDDIxNy4xOTUuMjguMCJJCQrXo3A9akhAEexRuB6F6wFAGgNGUkEiBEZSLUo6FEJvdWxvZ25lLUJpbGxhbmNvdXJ0QgU5MjEwMEgCUHhY9AriPgUI7eGoBHIHV2luZG93c5ABAuEBAAAAAAAA8D/6AYEBCh0KCENocm9taXVtEgMxNDASATASBDczMzkSAzIwOAoaCgtOb3Q9QT9CcmFuZBICMjQSATASATASATAKIgoNR29vZ2xlIENocm9tZRIDMTQwEgEwEgQ3MzM5EgMyMDgSEwoHV2luZG93cxICMTUSATASATAYACIDeDg2KgI2NDgC0kIAMpoMUuEFQ1FaVk9nQVFaVk9nQUFHQUJDRU5CLUZzQVBfZ0FFUGdBQXFJS2JNQmJHSk1TU0ZnY1RwMVlPc0FhWWdld1ZBQ3FzQWdCZ0tCQVFBQmlCSUN0SVFVa0dBQUlBaEFoaUFBQUFJQUNFQkFBQUJsQUFCQUFBQUFJQUFBQUNBTUNBQUFBQkFBSUNBQUFBQUJRZ0FBQ0VBSkFRQUFBQUFReEVCRUFBRUFnUUlFNkpwVXdBeEFBQUFBQ0FBQUFBQUFBQUNBQUFBSUFBQUFBQUFBQVFBQUFBZ0FBQUFBQUFJQUFBQUFBQkFBRUFBQUFBRUFBQUFBQUFBQUFBQUFBQUFBQUFBQUFBQUFFRElZQ1FBQ2dBSGdBa0FCVUFFY0FSd0E3Z0I5Z0VBQUlRQVJ3QV80Q1BRRXhBTEVBV29BeFlCa0lCSVNBNUFCUUFGUUFPQUFnZ0JrQUdvQVBBQW1BQlZBRGVBSDRBUWdBamdCV2dEREFHVUFPY0Fkd0Etd0ItZ0VBQU5FQWJRQlI0QzJBRjVnTU5nWkdCa2dEVndISmpvRU1BRkFBVkFBNEFDQ0FHUUFhZ0E4QUNZQUZXQUxnQXVnQmlBRGVBSDRBVVlBclFCaGdES0FIT0FPNEFmWUFfUUNMQUVkQU5vQW1RQlI0QzJBRjVnTDZBWWFBeDZCa1lHU0FOWEFjbUJIWWNBQkFDZ1FnRmdBVUFCcUFGVUFMZ0FZZ0EzZ0ItQUhPQU80QTJnREl5VUE4QUNnQUhBQWVBQk1BQ3FBRndBTVFBamdCUmdDdEFVZUFwb0JiQUM4NEdSZ1pJVWdOUUFVQUJVQURnQUlJQVpBQm9BRHdBSmdBVlFBeEFCLUFGR0FLMEFaUUE1d0ItZ0VXQUk2QWJRQlRRQzJBRjVnTDZBWWJBeU1ESkFISmdRNUFqc1ZBQWdBS0tBQXdBb0FCYkFJNFdnQmdBMUFIY0Fwb0FBQS5mX3dBQUFBQUFBQUH6PrIGChYSBe4DhhZZGg0xfjg5LjQ5NC4yODIyEuEFQ1FaVk9nQVFaVk9nQUFHQUJDRU5CLUZzQVBfZ0FFUGdBQXFJS2JNQmJHSk1TU0ZnY1RwMVlPc0FhWWdld1ZBQ3FzQWdCZ0tCQVFBQmlCSUN0SVFVa0dBQUlBaEFoaUFBQUFJQUNFQkFBQUJsQUFCQUFBQUFJQUFBQUNBTUNBQUFBQkFBSUNBQUFBQUJRZ0FBQ0VBSkFRQUFBQUFReEVCRUFBRUFnUUlFNkpwVXdBeEFBQUFBQ0FBQUFBQUFBQUNBQUFBSUFBQUFBQUFBQVFBQUFBZ0FBQUFBQUFJQUFBQUFBQkFBRUFBQUFBRUFBQUFBQUFBQUFBQUFBQUFBQUFBQUFBQUFFRElZQ1FBQ2dBSGdBa0FCVUFFY0FSd0E3Z0I5Z0VBQUlRQVJ3QV80Q1BRRXhBTEVBV29BeFlCa0lCSVNBNUFCUUFGUUFPQUFnZ0JrQUdvQVBBQW1BQlZBRGVBSDRBUWdBamdCV2dEREFHVUFPY0Fkd0Etd0ItZ0VBQU5FQWJRQlI0QzJBRjVnTU5nWkdCa2dEVndISmpvRU1BRkFBVkFBNEFDQ0FHUUFhZ0E4QUNZQUZXQUxnQXVnQmlBRGVBSDRBVVlBclFCaGdES0FIT0FPNEFmWUFfUUNMQUVkQU5vQW1RQlI0QzJBRjVnTDZBWWFBeDZCa1lHU0FOWEFjbUJIWWNBQkFDZ1FnRmdBVUFCcUFGVUFMZ0FZZ0EzZ0ItQUhPQU80QTJnREl5VUE4QUNnQUhBQWVBQk1BQ3FBRndBTVFBamdCUmdDdEFVZUFwb0JiQUM4NEdSZ1pJVWdOUUFVQUJVQURnQUlJQVpBQm9BRHdBSmdBVlFBeEFCLUFGR0FLMEFaUUE1d0ItZ0VXQUk2QWJRQlRRQzJBRjVnTDZBWWJBeU1ESkFISmdRNUFqc1ZBQWdBS0tBQXdBb0FCYkFJNFdnQmdBMUFIY0Fwb0FBQS5mX3dBQUFBQUFBQUEqNAoKcHViY2lkLm9yZxImCiQ1YWM0YTI4ZS04NThlLTRiYWItOTJkYS00OTA1NjdmMTMyYmM4AUDeAloDRVVSWgNVU0RiB0lBQjExLTRiB0lBQjEzLTJiB0lBQjE0LTFiB0lBQjE1LTFiCElBQjE5LTMwYgVJQUIyM2IHSUFCMjMtMWIISUFCMjMtMTBiB0lBQjIzLTJiB0lBQjIzLTNiB0lBQjIzLTRiB0lBQjIzLTViB0lBQjIzLTZiB0lBQjIzLTdiB0lBQjIzLThiB0lBQjIzLTliB0lBQjctMzliB0lBQjctNDRyBxIAyj4CCAGaATIiLQgBEiQKCmdvb2dsZS5jb20SFHB1Yi04MTQwMzQ2ODI2MDk4NDQzMAEaAzEuMJpCAKgBAdI/VRJKQUQ4RmRtNDFiNFVrTVJRUDhQR3VidlhXcnJiQzZGbzhoS0YzcDNtUHFreW9BNVFvYnczT3hkSC0tblBhQ0Q0U3l4OVZ1ZXEzTncgASoFMgEDOAA=
--------------------------------------------------
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

--------------------------------------------------
BidRequest.user.eids count: 0
BidRequest.user.ext.eids count: 0
==================================================
==================================================
Protobuf: ChZEX2NEZmpvek1UNHV6d19OMmZvWkZRErYCCgExEisIrAIQ+gEgA1isAmD6AWigAXAuegYIrAIQ+gGyPw0SCwigARCsAhguIPoBIgZHT09HTEU6CTQ3MDY5MTEzMUEAAAAAAAD0P0oDRVVSWjEIARItCgcxODIxODQ5EQAAAAAAAAhAGgNFVVIwA5I/EQgBGAEqAwoBATIG7YnP4cgDYAFwkByKAScKEmNsaWNrX3Rocm91Z2hfcmF0ZREAAADg7xmAPxoIRVhDSEFOR0WKASAKC3ZpZXdhYmlsaXR5ERSuR+F6FO4/GghFWENIQU5HRYo/XQjtic/hyAMRVIs2fP/VXk4Ro+tsJsNqTQMRgNnIk0DVDYgaAQBAA0oiEiAyZWM1MWE1NGE1NmI0ZDllYjNmYjg3MTZjYjZkNTI4ZGghaCB6BggCEAEYAZABAJAEABrDATo7aHR0cHM6Ly9jdWlzaW5lLmpvdXJuYWxkZXNmZW1tZXMuZnIvcmVjZXR0ZS8zMjczNDgtcGFuY2FrZXNaHQoUcHViLTc0MTgxOTE3MjgyMTE2MjbSPgQKAkZSYlJoAJoBAmZysgEoRGVzc2VydHMgYW5kIEJha2luZyxGb29kICYgRHJpbmssQ29va2luZ+IBHQoBMBoFCgMyMTAaBQoDMjE3GgUKAzIxNsI+AggGeAGSPw4IABABIgAqBAgBEAAwASqVAxKQAU1vemlsbGEvNS4wIChpUGhvbmU7IENQVSBpUGhvbmUgT1MgMjZfMF8xIGxpa2UgTWFjIE9TIFgpIEFwcGxlV2ViS2l0LzYwNS4xLjE1IChLSFRNTCwgbGlrZSBHZWNrbykgR1NBLzM5MC4wLjgxNzM4ODY0NiBNb2JpbGUvMTVFMTQ4IFNhZmFyaS82MDQuMRoLMTk2Ljc1LjY2LjAiLQn2KFyPwvVAQBGamZmZmZkbwBoDTUFSIgVNQS0wN0gCUDxYoSPiPgUIp+ipBGIFQXBwbGVqBmlwaG9uZXIDaU9TegYyNi4wLjGQAQTIAcAC0AG1BeEBAAAAAAAACED6AY4BCg8KB01vemlsbGESATUSATAKGQoLQXBwbGVXZWJLaXQSAzYwNRIBMRICMTUKGAoDR1NBEgMzOTASATASCTgxNzM4ODY0NgoQCgZNb2JpbGUSBjE1RTE0OAoQCgZTYWZhcmkSAzYwNBIBMRISCgZpUGhvbmUSAjI2EgEwEgExGAEqAjY0MgZpUGhvbmU4A9JCADKNAUIPCgEwGgUKAzM2OMI+AggEWkQKFHB1Yi03NDE4MTkxNzI4MjExNjI2EiwKIE1UaDBjMmMzTnpFNGRITm5OemN4T0hSelp6YzNNVGgwyj4HCgVwcHVpZPo+MyoxCgtmaXJzdC1pZC5mchIiCiAyZWM1MWE1NGE1NmI0ZDllYjNmYjg3MTZjYjZkNTI4ZDgBQN4CWgNFVVJaA1VTRGIHSUFCMTEtNGIHSUFCMTItMWIHSUFCMTItMmIHSUFCMTItM2IHSUFCMTMtMmIHSUFCMTMtM2IHSUFCMTQtMWIISUFCMTUtMTBiB0lBQjctMzmaATIiLQgBEiQKCmdvb2dsZS5jb20SFHB1Yi03NDE4MTkxNzI4MjExNjI2MAEaAzEuMJpCAKgBAdI/UhJKQUQ4RmRtN0g1VmN6YWVoajRlS1dobmdMNVNtd3Q3cGpKbWhZaWJPbGtnUmFQbnE3X3pXNEhabUZIVkdQY2hoRm5iYlA0U1BQMkEgASoCOAE=
--------------------------------------------------
id: "D_cDfjozMT4uzw_N2foZFQ"
imp {
  id: "1"
  banner {
    w: 300
    h: 250
    pos: BELOW_THE_FOLD
    wmax: 300
    hmax: 250
    wmin: 160
    hmin: 46
    format {
      w: 300
      h: 250
    }
    1014: {
      2: {
        1: 160
        2: 300
        3: 46
        4: 250
      }
    }
  }
  displaymanager: "GOOGLE"
  tagid: "470691131"
  bidfloor: 1.25
  bidfloorcur: "EUR"
  pmp {
    private_auction: true
    deals {
      id: "1821849"
      bidfloor: 3.0
      bidfloorcur: "EUR"
      at: FIXED_PRICE
      1010: {
        1: 1
        3: 1
        5: {
          1: "\001"
        }
        6: "\355\211\317\341\310\003"
      }
    }
  }
  secure: true
  exp: 3600
  metric {
    type: "click_through_rate"
    value: 0.007861970923841
    vendor: "EXCHANGE"
  }
  metric {
    type: "viewability"
    value: 0.94
    vendor: "EXCHANGE"
  }
  1009: {
    1: 122611287277
    2: 0x4e5ed5ff7c368b54
    2: 0x034d6ac3266ceba3
    2: 0x880dd54093c8d980
    3: "\000"
    8: 3
    9: {
      2: "2ec51a54a56b4d9eb3fb8716cb6d528d"
    }
    13: 33
    13: 32
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
  page: "https://cuisine.journaldesfemmes.fr/recette/327348-pancakes"
  publisher {
    id: "pub-7418191728211626"
    1002: {
      1: "FR"
    }
  }
  content {
    livestream: false
    language: "fr"
    genre: "Desserts and Baking,Food & Drink,Cooking"
    data {
      id: "0"
      segment {
        id: "210"
      }
      segment {
        id: "217"
      }
      segment {
        id: "216"
      }
      1000: {
        1: 6
      }
    }
  }
  mobile: true
  1010: {
    1: 0
    2: 1
    4: {
    }
    5: {
      1: 1
      2: 0
    }
    6: 1
  }
}
device {
  ua: "Mozilla/5.0 (iPhone; CPU iPhone OS 26_0_1 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) GSA/390.0.817388646 Mobile/15E148 Safari/604.1"
  ip: "196.75.66.0"
  geo {
    lat: 33.92
    lon: -6.9
    country: "MAR"
    region: "MA-07"
    type: IP
    utcoffset: 60
    accuracy: 4513
    1004: {
      1: 9073703
    }
  }
  make: "Apple"
  model: "iphone"
  os: "iOS"
  osv: "26.0.1"
  devicetype: HIGHEND_PHONE
  w: 320
  h: 693
  pxratio: 3.0
  sua {
    browsers {
      brand: "Mozilla"
      version: "5"
      version: "0"
    }
    browsers {
      brand: "AppleWebKit"
      version: "605"
      version: "1"
      version: "15"
    }
    browsers {
      brand: "GSA"
      version: "390"
      version: "0"
      version: "817388646"
    }
    browsers {
      brand: "Mobile"
      version: "15E148"
    }
    browsers {
      brand: "Safari"
      version: "604"
      version: "1"
    }
    platform {
      brand: "iPhone"
      version: "26"
      version: "0"
      version: "1"
    }
    mobile: true
    bitness: "64"
    model: "iPhone"
    source: USER_AGENT_STRING
  }
  1066: {
  }
}
user {
  data {
    id: "0"
    segment {
      id: "368"
    }
    1000: {
      1: 4
    }
  }
  eids {
    source: "pub-7418191728211626"
    uids {
      id: "MTh0c2c3NzE4dHNnNzcxOHRzZzc3MTh0"
      1001: {
        1: "ppuid"
      }
    }
  }
  1007: {
    5: {
      1: "first-id.fr"
      2: {
        1: "2ec51a54a56b4d9eb3fb8716cb6d528d"
      }
    }
  }
}
at: FIRST_PRICE
tmax: 350
cur: "EUR"
cur: "USD"
bcat: "IAB11-4"
bcat: "IAB12-1"
bcat: "IAB12-2"
bcat: "IAB12-3"
bcat: "IAB13-2"
bcat: "IAB13-3"
bcat: "IAB14-1"
bcat: "IAB15-10"
bcat: "IAB7-39"
source {
  schain {
    complete: true
    nodes {
      asi: "google.com"
      sid: "pub-7418191728211626"
      hp: true
    }
    ver: "1.0"
  }
  1059: {
  }
}
cattax: IAB_CONTENT_1_0
1018: {
  2: "AD8Fdm7H5Vczaehj4eKWhngL5Smwt7pjJmhYibOlkgRaPnq7_zW4HZmFHVGPchhFnbbP4SPP2A"
  4: 1
  5: {
    7: 1
  }
}

--------------------------------------------------
BidRequest.user.eids count: 1
BidRequest.user.ext.eids count: 0
==================================================
```
