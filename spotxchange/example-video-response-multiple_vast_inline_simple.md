# Video Bid Response With Inline VAST, Non-VPAID

Following is an example of a bid response that returns multiple
Seat Bid objects in response to a Bid Request containing multiple
impressions. A few notes about the example:

### Seat 164
This bidder is presenting a group to be taken as a whole or not at
all. Impression IDs 1 and 2 are associated with a deal, while 3
is not. Also, all Bid objects will return VAST ad markup via their
win notice URL.

### Seat 45
This bidder is bidding on only Impression ID 2 and has associated
it with a deal. They provide both a win notice URL and the VAST
ad markup inline.

### Seat 33
They are providing bids for 3 Impressions and will accept any or
all be selected. Also, all Bid objects will return VAST ad markup
via their win notice URL.

With these three seats bidding across all impressions, the exchange
could choose to:

- Take **all** of Seat 165's bids,
- Take **all** of Seat 33's bids, or
- **Mix** Seat 145's Impression ID 2 bid with Seat 33's Impression ID
1 and 3 bids, since Seat 145 indicated they would be willing to pay
more for Impression ID 2 than Seat 145 did.

It should be noted that for all VAST documents returned via win
notices that the exchange will be making extra HTTP round trips
during playback. Given that VAST documents can be substantial and
video assets as well, each exchange should be sure to take this
into account for user experience timing.

```json
{
  "id": "123",
  "seatbid": [
    {
      "seat": "165",
      "group": 1,
      "bid": [
        {
          "id":      "12345-1",
          "impid":   "1",
          "price":   600,
          "dealid":  "1452f.eadb4.7aaa",
          "nurl":    "http://example165.com/win/12345-1?won=${AUCTION_PRICE}"
        },
        {
          "id":      "12345-2",
          "impid":   "2",
          "price":   300,
          "dealid":  "1452f.eadb4.f9bc",
          "nurl":    "http://example165.com/win/12345-2?won=${AUCTION_PRICE}"
        },
        {
          "id":     "12345-3",
          "impid":  "3",
          "price":  300,
          "nurl":   "http://example165.com/win/12345-3?won=${AUCTION_PRICE}"
        }
      ]
    },

    {
      "seat": "45",
      "bid": [
        {
        "id":      "0123456789ABCDEF0123456789ABCDEF",
        "impid":   "2",
        "price":   300,
        "dealid":  "1452f.eadb4.7aaa",
        "nurl":    "http://example45.com/win/0123456789ABCDEF0123456789ABCDEF?won=${AUCTION_PRICE}",

        "adm": "%3C%3Fxml%20version%3D%221.0%22%20encoding%3D%22utf-8%22%3F%3E%0A%3CVAST%20version%3D%222.0%22%3E%0A%20%20%20%20%3CAd%20id%3D%2212345%22%3E%0A%20%20%20%20%20%20%20%20%3CInLine%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%3CAdSystem%20version%3D%221.0%22%3ESpotXchange%3C%2FAdSystem%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%3CAdTitle%3E%3C!%5BCDATA%5BSample%20VAST%5D%5D%3E%3C%2FAdTitle%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%3CImpression%3Ehttp%3A%2F%2Fsample.com%3C%2FImpression%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%3CDescription%3E%3C!%5BCDATA%5BA%20sample%20VAST%20feed%5D%5D%3E%3C%2FDescription%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%3CCreatives%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%3CCreative%20sequence%3D%221%22%20id%3D%221%22%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%3CLinear%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%3CDuration%3E00%3A00%3A30%3C%2FDuration%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%3CTrackingEvents%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%3C%2FTrackingEvents%3E%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%3CVideoClicks%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%3CClickThrough%3E%3C!%5BCDATA%5Bhttp%3A%2F%2Fsample.com%2Fopenrtbtest%5D%5D%3E%3C%2FClickThrough%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%3C%2FVideoClicks%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%3CMediaFiles%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%3CMediaFile%20delivery%3D%22progressive%22%20bitrate%3D%22256%22%20width%3D%22640%22%20height%3D%22480%22%20type%3D%22video%2Fmp4%22%3E%3C!%5BCDATA%5Bhttp%3A%2F%2Fsample.com%2Fvideo.mp4%5D%5D%3E%3C%2FMediaFile%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%3C%2FMediaFiles%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%3C%2FLinear%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%3C%2FCreative%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%3C%2FCreatives%3E%0A%20%20%20%20%20%20%20%20%3C%2FInLine%3E%0A%20%20%20%20%3C%2FAd%3E%0A%3C%2FVAST%3E"
        }
      ]
    },

    {
      "seat": "33",
      "bid": [
        {
          "id":      "388f110487f9d1e63f77b22d86e916b",
          "impid":   "1",
          "price":   600,
          "dealid":  "1452f.eadb4.7aaa",
          "nurl":    "http://example33.com/win/DECAFBAD?won=${AUCTION_PRICE}"
        },
        {
          "id":      "388f110487f9d1e63f77b22d86e916c",
          "impid":   "2",
          "price":   260,
          "dealid":  "1452f.eadb4.f9bc",
          "nurl":    "http://example33.com/win/CAFEBABE?won=${AUCTION_PRICE}"
        },
        {
          "id":     "388f110487f9d1e63f77b22d86e916d",
          "impid":  "3",
          "price":  300,
          "nurl":   "http://example33.com/win/DEADBEEF?won=${AUCTION_PRICE}"
        }
      ]
    }
  ]
}
```
