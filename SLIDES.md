title: Openbanking APIs
author:
  name: Antanas Sinica, Mindaugas Žilinskas
  url: https://www.swedbank.com/openbanking/
output: slides.html
controls: true

--

# Bankų Open API - žvilgsnis po kapotu
## Antanas Sinica, Mindaugas Žilinskas

--

### Intro

API Scope; Ownership


--
### PSD2 - Payment Service Directive 2
Players:
PSU - end user
TPP - third party provider
Bank

<<diagram - PSU -> TPP -> *multibank>>
--
### Message
* Main message: Customer is owner of account, not the bank.
* User can use any intermediate as a channel or product provider to use accounts
* PSD2 services are provided on same cost basis as 
* e.g. U may "add" your account from X bank to Swedbank internet bank and use as native one
* e.g. ERP may create integration to any bank
--
### Endpoints
* /consent/
* /accouts/
* /payments/

* integration pattern - redirect, later decoupled/embedded
--

### General schema - 1
* Preauth OAuth2 based schema
1. if U have OAuth2 token - call backend
2. if not get it using OAuth2 "Authorization code" grant 
3. if U get error on expired/ non-existant token - refresh


--
### General schema - 2 
* General flow for AIS + PIS flow
1. TPP->Bank: request account list consent  POST /consent/ {accounts:allAccounts}
2. Bank->PSU: SCA on this item redirect to flow
3. TPP: /accounts/
4. TPP: prepare consent POST /consent/ { balances{}, transactions {} }
5. Bank->PSU: SCA on this item redirect to flow
6. TPP: /accounts/ + /accounts/${id}/balances  + /accounts/${id}/transactions
7. TPP: prepare payment + POST/payment/ XML or json
8. Bank->PSU: SCA on this item redirect to flow
9. TPP: check payment status
10. TPP: check status:  /accounts/${id}/balances  + /accounts/${id}/transactions


--

### API Management

* Item 1
* Item B
* Item gamma

New Paragraph

--

### API Aggregation

Opening statement: 6000+ finansinių institucijų europoje affected

Level 2: UK susitarė atskirai, France irgi, Poland irgi, Berlin-group inception

Level 3: Tas daro XML, anas XML ir panašiau į SOAP, šitas JSON, dar kitas dar kažkaip JSON

level 4: PSD2 evolving standard in English; Berlin-group spec evolving standard also in English; RESTful JSON APIs trying to keep up with all the English - no standards.

Level 5: JSON APIs examples; v1, √2, v3;

Level 6: ... 

--

### Outro

Selling points, Fintech map

### PSD2 and security solution around it

general story on SCA + OAuth2 code path
OWASP for OAuth2

### PSD2 and security solution around it

Mutual SSL and signing of messages

### HSM

Ideas about pkey protection
