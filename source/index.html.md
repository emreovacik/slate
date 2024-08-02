---
title: Sendrater API Reference

language_tabs: # must be one of https://github.com/rouge-ruby/rouge/wiki/List-of-supported-languages-and-lexers
  - shell
  - go

toc_footers:
  - <a href='https://www.sendrater.com/register'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true

code_clipboard: true

meta:
  - name: description
    content: Sendrater API Documentation
---

# Introduction

Welcome to the Sendrater API! You can use our API to access remittance market rates' endpoints.

To gain access to API endpoints; please create your subscription at sendrater.com and create your API token from the panels. Once you have your API token, you can call API endpoints using it as your authorization key, in the header part of your requests. You may find request codes and response samples in this document. You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

# Authentication

> To authorize your requests, you need to specify your access token in the header part of your request:

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here" \
  -H "Authorization: API_TOKEN"
```

```go
req, err := http.NewRequest(method, url, payload)
req.Header.Add("Authorization", "API_TOKEN")
```

> Make sure to replace `API_TOKEN` with your API token acquired from the sendrater panel.

Sendrater API expects for the token to be included in all API requests to the server in a header that looks like the following:

`Authorization: API_TOKEN`

# Endpoints

## Get Origin Country List

```shell
curl --location 'https://api.sendrater.com/country/from' \
--header 'Authorization: ••••••'
```

```go
func main() {
  url := "https://api.sendrater.com/country/from"

  client := &http.Client{}
  req, err := http.NewRequest("GET", url, nil)

  if err != nil {
    fmt.Println(err)
    return
  }
  req.Header.Add("Authorization", "••••••")

  res, err := client.Do(req)
  if err != nil {
    fmt.Println(err)
    return
  }
  defer res.Body.Close()

  body, err := ioutil.ReadAll(res.Body)
  if err != nil {
    fmt.Println(err)
    return
  }
  fmt.Println(string(body))
}
```

> The above command returns origin country list JSON structured like this:

```json
[
    {
        "code": "DEU",
        "name": "Germany",
        "shortCode": "DE"
    },
    {
        "code": "GBR",
        "name": "United Kingdom",
        "shortCode": "GB"
    },
    {
        "code": "USA",
        "name": "United States",
        "shortCode": "US"
    }
]
```

You can get the available origin country list from the endpoint:

### HTTP Request

`GET https://api.sendrater.com/country/from`

## Get Destination Country List

```shell
curl --location 'https://api.sendrater.com/country/to/<ORIGIN_COUNTRY_CODE>' \
--header 'Authorization: ••••••'
```

```go
func main() {
  url := "https://api.sendrater.com/country/to/<ORIGIN_COUNTRY_CODE>"

  client := &http.Client{}
  req, err := http.NewRequest("GET", url, nil)

  if err != nil {
    fmt.Println(err)
    return
  }
  req.Header.Add("Authorization", "••••••")

  res, err := client.Do(req)
  if err != nil {
    fmt.Println(err)
    return
  }
  defer res.Body.Close()

  body, err := ioutil.ReadAll(res.Body)
  if err != nil {
    fmt.Println(err)
    return
  }
  fmt.Println(string(body))
}
```

> The above command returns destination country list JSON structured like this:

```json
[
    {
        "code": "ALB",
        "name": "Albania",
        "shortCode": "AL"
    },
    {
        "code": "ARG",
        "name": "Argentina",
        "shortCode": "AR"
    },
    {
        "code": "ARM",
        "name": "Armenia",
        "shortCode": "AM"
    }
]
```

You can get the available destination country list from this endpoint.

### HTTP Request

`GET https://api.sendrater.com/country/to/<ORIGIN_COUNTRY_CODE>`

Replace `<ORIGIN_COUNTRY_CODE>` with 3 letter ISO country code.


### URL Parameters

Parameter | Description
------------------- | -----------
ORIGIN_COUNTRY_CODE | 3 letter ISO country code

## Marketplace Rates

```shell
curl --location 'https://api.sendrater.com/rates' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--header 'Authorization: ••••••' \
--data-urlencode 'f=GBR' \
--data-urlencode 't=IND' \
--data-urlencode 'a=100'
```

```go
func main() {

  url := "https://api.sendrater.com/rates"

  payload := strings.NewReader("f=GBR&t=IND&a=100")

  client := &http.Client {
  }
  req, err := http.NewRequest("POST", url, payload)

  if err != nil {
    fmt.Println(err)
    return
  }
  req.Header.Add("Content-Type", "application/x-www-form-urlencoded")
  req.Header.Add("Authorization", "••••••")

  res, err := client.Do(req)
  if err != nil {
    fmt.Println(err)
    return
  }
  defer res.Body.Close()

  body, err := ioutil.ReadAll(res.Body)
  if err != nil {
    fmt.Println(err)
    return
  }
  fmt.Println(string(body))
}
```

> The above command returns JSON structured like this:

```json
[
  {
    "src": {
        "id": 28,
        "name": "Zing"
    },
    "rate": 106.643,
    "fee": 0.02,
    "receive": 1064.29
  },
  {
    "src": {
        "id": 2,
        "name": "TapTapSend"
    },
    "rate": 106,
    "fee": 0,
    "receive": 1060
  }
]
```

This endpoint lists the providers with their current rates, fees and a receive value for the specified amount. If no amount specified, 100 is used.

### HTTP Request

`POST https://api.sendrater.com/rates`

### URL Parameters

Parameter | Mandatory | Description
--------- | ----------- | ------------
f | true | 3 letter ISO - FROM country code
t | true | 3 letter ISO - TO country code
a | false | Amount to send (100 if none specified) 

