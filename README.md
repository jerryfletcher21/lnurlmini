# lnurlmini

Minimal and with as few dependencies as possible python scripts for
[lnurl](https://github.com/lnurl/luds) client side.

All requests are made with tor.

Scripts are self contained, so they can be copied individually
everywhere.

## Pay

Decode an lnurl bech32/url/protocol/address, make the lnurl pay requests
and get an invoice.

Supports:
* [06](https://github.com/lnurl/luds/blob/luds/06.md)
  `payRequest` base spec
* [12](https://github.com/lnurl/luds/blob/luds/12.md)
  comments in `payRequest`
* [16](https://github.com/lnurl/luds/blob/luds/16.md)
  paying to static internet identifiers
* [17](https://github.com/lnurl/luds/blob/luds/17.md)
  scheme prefixes and raw (non bech32-encoded) URLs
* [lightning address](https://github.com/andrerfneves/lightning-address/blob/main/DIY.md)

Useful for who runs a lightning node without any external tools, and
wants to pay lnurls, ln addresses.

requirements: `requests[socks]`
```
lnurl-pay-get-invoice [-v | -vv] [-d, --decode] data [amount] [comment]

data can be:
    invoice normal, invoice protocol, lnurl bech32,
    lnurl url, lnurl protocol, lnurl lightning address

if data is not invoice amount should be present
if service allows comments, comment can be present

if --decode, simply decode the url, without making the requests
```

## Auth

Decode the lnurl bech32/url/protocol, make the lnurl auth requests to
authenticate.

Supports:
* [04](https://github.com/lnurl/luds/blob/luds/04.md)
  `auth` base spec
* [13](https://github.com/lnurl/luds/blob/luds/13.md)
  `signMessage`-based seed generation for `auth` protocol
* [17](https://github.com/lnurl/luds/blob/luds/17.md)
  scheme prefixes and raw (non bech32-encoded) URLs

Useful for authenticating in websites directly from the terminal without
needing extra tools/wallets.

In order to support signMessage-based seed generation for auth protocol,
after obtaining the signature from the lightning node as explained
[here](https://github.com/lnurl/luds/blob/luds/13.md), do
```
cat <signature-file> | lnurl-auth --hex lnurl
```

requirements: `requests[socks]` `ecdsa`
```
lnurl-auth [-v | -vv] [--linking-key] [--hex] [--no-hmac] data

data can be:
    lnurl bech32, lnurl url, lnurl protocol

in stdin it should be present the secret/linking-key

if --linking-key, directly use the string passed in stdin as linking-key
if --hex, treat the string in stdin as an hexadecimal
if --no-hmac, do not hmac with the url
```

## License

lnurlmini is released under the terms of the ISC license.
See [LICENSE](LICENSE) for more details.
