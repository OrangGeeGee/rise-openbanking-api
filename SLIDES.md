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
### before PSD2 - Payment Service Directive 2
<span>
<img alt="PSU (end user/customer) uses TPPs (third party provider) to work with Banks" height="380" width="380" src="file://C:/Users/minzli/slides_rise/1before.svg" />
</span>

--
### after PSD2 - Payment Service Directive 2

<span>
<img alt="PSU (end user/customer) uses TPPs (third party provider) to work with Banks" height="380" width="380" src="file://C:/Users/minzli/slides_rise/2after.svg" />
</span>

--
### PSD2 in human terms
#### Main message - Customer is owner of account, so Customer can use any intermediate as a channel or product provider to use his own accounts
#### PSD2 services are provided on same cost basis as main channel (bank is not charging extra from TPP) 
#### TPP has no agreement with bank
 
--
### PSD2 in technical terms
##### customer auth methods - <b>redirect</b>, later decoupled / embedded
##### pre-authenticate step (OAuth2 with grant code flow)
##### API based on Berlin Group Specification
* /consents/
* /accouts/${id}/balance, /accouts/${id}/transactions
* /payments/sepa-credit-transfer

--

### General schema 

<span>
<img alt="PSU (end user/customer) uses TPPs (third party provider) to work with Banks" height="380" width="380" src="file://C:/Users/minzli/slides_rise/3flow.svg" />
</span>

--

# Swedbank API
## Mutual TLS (QWAC), OAuth2, Signing (QSEALC), Redirect flow

--
### Security architecture
#### Can anyone call bank API to get any body's any account data or execute any payment?
* Not anyone - use mutual TLS based on QWAC as proof of certified TPP 
* Not anybody - use SCA based OAuth2 as auth engine for proving Customer identity
* Not any account - customer must give consent (use SCA to sign it).
* Not any payment- customer must sign payment (use SCA to sign it).
-- 
### Security architecture
#### Can certified TPP call bank API to get your allowed accounts data & initiate payment?
* Yes :)

#### Can TPP misbehave? Or be hacked?
* QSealC for payments signing
* OAuth2 token mapping to TLS
* short token lifetime
* anti fraud systems 
* ...
--
### Security architecture
<span>
<img alt="PSU (end user/customer) uses TPPs (third party provider) to work with Banks" height="380" width="380" src="file://C:/Users/minzli/slides_rise/4security.svg" />
</span>
--
### Oauth2 schema
10. TPP: check status:  /accounts/${id}/balances  + /accounts/${id}/transactions
<<diagram>>

--
### Payments schema
10. TPP: check status:  /accounts/${id}/balances  + /accounts/${id}/transactions
<<diagram>>

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
