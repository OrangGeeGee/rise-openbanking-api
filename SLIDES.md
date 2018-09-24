title: Openbanking APIs
author:
  name: Antanas Sinica, Mindaugas Žilinskas
  url: https://www.swedbank.com/openbanking/
output: slides.html
controls: true

--

# Banks' Open API - behind the scenes
## Antanas Sinica, Mindaugas Žilinskas

--

### Today

* PSD2 - what is it?
* Swedbank API
* Consuming the APIs
* Your turn ʕ·͡ᴥ·ʔ

--

# PSD2 - what is it?

--
### PSD2 - Payment Service Directive 2
Players:
PSU - end user / customer
TPP - third party provider
Bank

```
<<diagram - PSU -> multibank>>

<<diagram - PSU -> TPP -> *multibank>>
```

--
### PSD2 in human terms
* Main message: Customer is owner of account, not the bank, so Customer can use any intermediate as a channel or product provider to use accounts
* PSD2 services are provided on same cost basis as main channel (bank is not allowed to charge extra from TPP) 
* e.g. U may "add" your account from X bank to Swedbank internet bank and use as native one
* e.g. ERP may create integration to any bank

--
### PSD2 in technical terms
Pre-authenticate step (OAuth2 grant code flow)
* /consent/
* /accouts/
* /accouts/${id}/balance
* /accouts/${id}/transactions
* /payments/sepa-credit-transfer
* customer auth methods - redirect, later decoupled/embedded

--

### General schema - 1
* Preauth OAuth2 based schema
1. if U have OAuth2 token - call backend
2. if not get it using OAuth2 "Authorization code" grant 
3. if U get error on expired/ non-existant token - refresh
```<<diagram>>```

--

# Swedbank API
## Mutual TLS (QWAC), OAuth2, Signing (QSEALC), Redirect flow

--
### Security architecture

What if legal TPP (bad guy) from Bulgaria calls our API?

mutual SSL , Login server, JWT, OAuth2
<<diagram>>

Detailed flow
<<diagram>>
--
### PSD2 and security solution around it
general story on SCA + OAuth2 code path
OWASP for OAuth2

Mutual SSL and signing of messages

Ideas about pkey protection



--
### General schema - 2 
10. TPP: check status:  /accounts/${id}/balances  + /accounts/${id}/transactions
<<diagram>>

--

sequenceDiagram
    participant PSU as Customer
    participant TPP as Third Party Provider
    participant API as Bank API
    participant Login as Bank Login&Sign servers
    PSU->>TPP: login

    loop get OAuth2 token
         TPP->>API: pre-authentificate OAuth2.0
    end

    TPP->>+API: POST /consent {allAccounts}
    API->>-TPP: link for PSU to sign consent
    TPP->>PSU: redirect
    Activate PSU
    PSU->>+Login: view & sign list of visible accounts
    Login->>-PSU: redirect to TPP
    PSU->>TPP: consent OK
   Deactivate PSU

    TPP->>+API: /accounts
    API->>-TPP: { {"iban":"LT71"},{"iban":"LT72"},{"iban":"LT73"}}
    TPP->>PSU: show consent
    PSU->>TPP: provide consent details

    TPP->>API: POST /consent { "balance": ["LT71","LT72","LT73"], "transactions":["LT71"]}
    API->TPP: link for PSU to sign detailed consent
    PSU->>Login: sign consent
    PSU->>API: /accounts/
loop for each account
    PSU->>API: /accounts/${iban}/balance
    PSU->>API: /accounts/${iban}/transactions
end;
    TPP->>API: POST /consent {detailed consent}
    TPP->>API: POST /consent {detailed consent}
---

sequenceDiagram
    participant PSU as Customer
    participant TPP as Third Party Provider
    participant API as Bank API
    participant Login as Bank Login&Sign servers
    TPP->>API: pre-authentificate OAuth2.0

    TPP->>API: get list of accounts - POST /consent {allAccounts}
	TPP->>API: set detailed consent - POST /consent { "balance": ["LT71","LT72","LT73"], "transactions":["LT71"]}
	TPP->>API: show/ use balance/transactions - GET /accounts/${iban}/balance | /transactions
	TPP->>API: make payments - /POST/payment/

    API->>-TPP: link for PSU to sign consent
    TPP->>PSU: redirect
    Activate PSU
    PSU->>+Login: view & sign list of visible accounts
    Login->>-PSU: redirect to TPP
    PSU->>TPP: consent OK
   Deactivate PSU

    TPP->>+API: /accounts
    API->>-TPP: { {"iban":"LT71"},{"iban":"LT72"},{"iban":"LT73"}}
    TPP->>PSU: show consent
    PSU->>TPP: provide consent details

    TPP->>API: POST /consent { "balance": ["LT71","LT72","LT73"], "transactions":["LT71"]}
    API->TPP: link for PSU to sign detailed consent
    PSU->>Login: sign consent
    PSU->>API: /accounts/
loop for each account
    PSU->>API: /accounts/${iban}/balance
    PSU->>API: /accounts/${iban}/transactions
end;
    TPP->>API: POST /consent {detailed consent}
    TPP->>API: POST /consent {detailed consent}




--

# Consuming the APIs

--

### API Aggregation

Opening statement: 6000+ finansinių institucijų europoje affected

Level 2: UK susitarė atskirai, France irgi, Poland irgi, Berlin-group inception

Level 3: Tas daro XML, anas XML ir panašiau į SOAP, šitas JSON, dar kitas dar kažkaip JSON

level 4: PSD2 evolving standard in English; Berlin-group spec evolving standard also in English; RESTful JSON APIs trying to keep up with all the English - no standards.

Level 5: JSON APIs examples; v1, √2, v3;

Level 6: ... 

--

# Your turn ʕ·͡ᴥ·ʔ

--

### Upcomming

1. 2019 September 
2. Fintech map
3. Fun challenges on both sides of the fence
