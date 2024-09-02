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

Welcome to the Sendrater API! Use our API to access international money transfer rates and fee information from various providers.

To access the API endpoints, [sign up for an account](https://www.sendrater.com/register) at Sendrater and create your API token in the developer dashboard. After generating your API token, include it as your authorization key in the header of your requests. You will find request codes and response samples in this document. View code examples in the dark area to the right, and switch the programming language of the examples with the tabs in the top right.

# Authentication

> To authorize your requests, specify your access token in the header of your request:

```shell
# With shell, pass the correct header with each request
curl "api_endpoint_here" \
  -H "Authorization: API_TOKEN"
```

> Replace `API_TOKEN` with your API token from the Sendrater dashboard.

> Include your API token in all API requests to the server in a header formatted as:

`Authorization: API_TOKEN`

# Endpoints

## Get Origin Country List

> Retrieve the list of available origin countries for initiating money transfers.

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
  req.Header.Add("Authorization", "YOUR_API_TOKEN")

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

> This command returns a JSON structured like this:

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

Retrieve the list of available destination countries for sending money transfers based on the selected origin country.

```shell
curl --location 'https://api.sendrater.com/country/to/<ORIGIN_COUNTRY_CODE>' \
--header 'Authorization: YOUR_API_TOKEN'
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
  req.Header.Add("Authorization", "YOUR_API_TOKEN")

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

Replace `<ORIGIN_COUNTRY_CODE>` with a 3-letter ISO country code.


### URL Parameters

Parameter | Description
------------------- | -----------
ORIGIN_COUNTRY_CODE | 3 letter ISO country code

## Get Corridor Rates

Retrieve the current money transfer rates, fees, and expected receive amounts for different providers for a specific origin and destination country pair (corridor). The system updates the data several times a day to keep it as close to real-time as possible.

```shell
curl --location 'https://api.sendrater.com/rates' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--header 'Authorization: YOUR_API_TOKEN' \
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
  req.Header.Add("Authorization", "YOUR_API_TOKEN")

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

> This command returns a JSON structured like this:

```json
[
  {
    "src": {
        "id": 1,
        "name": "Provider 1"
    },
    "rate": 106.643,
    "fee": 0.02,
    "receive": 1064.29
  },
  {
    "src": {
        "id": 2,
        "name": "Provider 2"
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

## Use Cases

Use the Sendrater API to leverage near real-time data on international money transfer rates and fees to drive business growth and improve strategic decision-making. Here are five ways you can use the Sendrater API:

1. **Implement Dynamic Pricing Strategies**: Adjust your pricing models dynamically based on competitor data, market demand, and other factors. This approach helps you stay competitive and maximize conversion rates.

2. **Conduct Market Intelligence and Competitive Analysis**: Monitor competitor rates and analyze their reactions to market changes or your campaigns. Use this insight to refine your pricing strategy and promotional planning.

3. **Boost Customer Acquisition**: Offer users more transparent and competitive rates than other providers in the market. This strategy can help you attract new customers and retain existing ones.

4. **Plan for Expansion**: Analyze rate dynamics and the competitive landscape of new corridors you plan to enter. This data-driven approach supports informed decisions on market entry and growth strategies.

5. **Improve Operational Efficiency and Identify Issues**: Monitor rate changes across different corridors to quickly spot pricing discrepancies or potential issues. Ensure your platform consistently displays accurate and competitive rates.

**And Many More!** The flexibility of the Sendrater API allows you to apply it in various ways to suit your unique business needs and goals.