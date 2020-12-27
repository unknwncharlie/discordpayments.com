# Discord Payments

![](https://discordpayments.com/logo.png)

Discord Payments is a discord bot that allows for secure crypto currency payments to be made by the users on your discord server. 

Simply add the bot to your server and people can start making payments.

# How it works

The Discord Payments bot integrates with [CoinPayments](https://www.coinpayment.net/) to offer the ability to make payments in over 50 different crypto currencies.

Upon a completed payment what we do is return a unique signed receipt in the form of a JWT. This receipt contains:
  - The user that requested the payment
  - The server the payment was requested on
  - The amount in USD
  - A unique identifier to specify product (This is controlled on payment request)
  - The address the funds were sent too

Including the server, user, unique identifier and address prevents token re use against multiple different discord servers.

Once a payment is complete it will give the user a token. The user will then send this token to your bot which will be able to verify the payment using the token. It must then record that this token has been used to give access to whatever is being sold.

WE DO NOT TAKE AWAY THE NEED TO INCOORPORATE ACCESS CONTROL TO YOUR BOT WE SIMPLY MAKE THE PAYMENT SIDE SIMPLER!

Tokens do not expire and we suggest you run checks on tokens being used to avoid token reuse within the server. We provide enough information within the receipt token to allow access controls to be implemented. 

# Example
To interact with the bot use `$$` to specify you are running a command.

To make a payment you specify the buy command and pass 4 parameters.
```
$$[COMMAND] [AMOUNT IN USD] [CRYPTO CURRENCY] [ADDRESS TO SEND PAYMENT TOO] [UNIQUE IDENTIFIER]
```
For example: 
`$$buy 50 BTC 3QT3MUnfJQqj3wzAFtHL4GVKPZYk3tM4K1 1234`

The above command would return a link with details about the payment that needs to be made. Upon sending the correct amount of Bitcoin to the address in the payment details you will be redirected to a page with your token. An example is seen bellow:
![](https://discordpayments.com/example.png)

This token is evidence of a completed payment and can be verified in your own bot.

The address used is specified by server owner and the bot managing payments should check the receipt token that the address the funds have been sent too match.

# Usage
### $$buy
This command can be used to make a payment. You will be given a link to follow which will give you details about your payment.

```
$$buy [Amount in USD] [Crypto Currency to make payment] [Address to send payment too] [Unique Identifier]
```
An example can be seen bellow:
`$$buy 50 BTC 3QT3MUnfJQqj3wzAFtHL4GVKPZYk3tM4K1 1234`
You will be given a receipt token on successfully completing the transaction.
### $$verify
This command can be used to verify your receipt token. Only use this to verify your token is working and never to verify payments.
```
$$verify [Receipt Token]
```
### $$coins
Display information about the available coins supported by this bot.
### $$help
Display this message.
# Token Verfication
To verify a receipt token you need to use Discord Payments public key. This can be found bellow along with a python code snippet which will allow you to retireve the useful information from the token. Integrate this piece of code into your bot to verify a token is legitimate.

### Public Key
```
-----BEGIN PUBLIC KEY-----
MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAroDGdpPsGdP1DKz6OCSC
8uDEIXiaZwA04CFkH6XzyB1wClFbGUx1B/k+a87/7sOhRCgcMyjOmiV3iWUody3D
iJcpbbx+PGekVMf8qMDUJpYTQWOqau5hKcYr0WXw35JKPhqBQ7Kvlc4Q3RUlbtrL
mIjWfr6Vs04fXEY/J8Yqv8QXOOnAJSRG/WLsojBRTsHox7sXGtSHqVGVX7JPRBE2
mtj9NLTFscX3yo8C7Fx7GlKrSW6cDsv8BIsB7SiXBJF1pA5zoOroQEjJSP5Ugspi
Pccdxpv/Ea5DnR1GVe2Dekx844WH22ptWK55vQnroqiQDUj8/9C4n5I2EKJpgAUE
XQIDAQAB
-----END PUBLIC KEY-----
```

### Token Verfification
```python
import jwt

public_key = """... Public Key here ..."""

def verifyToken(encoded):
	try:
		decoded = jwt.decode(encoded, public_key, algorithms='RS256')
		return decoded
	except jwt.exceptions.InvalidSignatureError:
		print("Invalid Token")
		return False
	except jwt.exceptions.DecodeError:
		print("Invalid Format")
		return False
	except Exception as e:
		print("Something went wrong")
		return False

encoded = "... token here ..."
token = verifyToken(encoded)
if token:
	print(f"Amount: {token['amount']}") # token['amount']
	print(f"Identifier: {token['identifier']}") # token['identifier']
	print(f"Server: {token['server']}") # token['server']
	print(f"Username: {token['username']}") # token['username']
	print(f"Address: {token['address']}") # token['address']
```
Replace `print` statements for bot functionality.

# Installation

Use this link to add the [Official Discord Payments Bot to your Server](https://discord.com/api/oauth2/authorize?client_id=784856689337827359&permissions=67584&scope=bot).

### Discord
Join the [Official Discord Payments Community](https://discord.gg/EuYnVnCma7).
