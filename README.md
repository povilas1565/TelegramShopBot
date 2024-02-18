# TelegramShopBot
**TelegramShopBot is a software product based on Aiogram3.x and SQLAlchemy that allows you to automate sales of digital goods in Telegram. One of the advantages of the bot is that AiogramShopBot implements the ability to top up with Bitcoin, Litecoin, USDT TRC-20, which allows you to sell digital goods worldwide.**


## 1.Starting the bot
> **Note**
> **TelegramShopBot use Aiosqlite**
* Clone the project ``git clone https://github.com/povilas1565/TelegramShopBot.git``
* Install all necessary packages ``pip install -r requirements.txt``
* Set the environment variables to run in the .env file.<br>Example:
```
WEBHOOK_HOST = "www.my-bot-example.com"
WEBHOOK_PATH = "/bot"
WEBAPP_HOST = "localhost"
WEBAPP_PORT = 1234
TOKEN = "TELEGRAM_BOT_TOKEN_HERE"
ADMIN_ID_LIST = 123456,654321
SUPPORT_LINK = "https://t.me/your_username_123"
DB_NAME = "db_file_name.db"
```
* If you don't have your own webhook site, you can always use ngrok for this, in which case your .env will look like this.
```
WEBHOOK_HOST = "https://abcd-123-456-789-01.ngrok-free.app"
WEBHOOK_PATH = ""
WEBAPP_HOST = "localhost"
WEBAPP_PORT = 1234
TOKEN = "TELEGRAM_BOT_TOKEN_HERE"
ADMIN_ID_LIST = 123456,654321
SUPPORT_LINK = "https://t.me/your_username_123"
DB_NAME = "db_file_name.db"
```
* After these steps the bot is ready to run, the entry point to run the bot is run.py ```python run.py```
*
## 2.TelegramgramShopBot User's Manual
### 2.1 Registration
User registration occurs when the bot is first accessed with the ``/start`` command. Each user is assigned a different mnemonic phrase to generate BTC, LTC, USDT TRC20 crypto addresses. BTC and LTC addresses are generated according to BIP-84 standard, for USDT TRC20 the BIP-44 standard is used, this is done so that wallets can be easily imported into Trust Wallet.
### 2.2 Top-up balance
To deposit balance in the bot, go to "My Profile -> Top Up balance". Copy the address of the cryptocurrency you want to top up and send the cryptocurrency there, then go back to "My Profile" and click "Refresh Balance". Refreshing the balance takes some time (1-2 seconds).
>**Note**
> "Refresh balance" button has a 30 second cooldown.
### 2.3 Purchase of goods
To buy any item, go to "All categories" -> Select any category -> Select any subcategory -> Select quantity -> Confirm purchase. If the purchase is successful, you will immediately receive a message in the format:
```
Item#1
Data: DataOfItem#1
Item#2
Data:DataOfItem#2
Item#3
Data:DataOfItem#3
Item#4
Data:DataOfItem#4
Item#5
Data:DataOfItem#5
Item#6
Data:DataOfItem#6
Item#7
Data:DataOfItem#7
Item#8
Data:DataOfItem#8
Item#9
Data:DataOfItem#9
Item#10
Data:DataOfItem#10
```
### 2.4 Purchase history
* To access your purchase history go to "My Profile" -> "Purchase History".
* You will be presented with an inline keyboard with all your purchases, by clicking on any of the purchases you will be sent a message in the format from paragraph 2.3.
## 3.TelegramShopBot Admin Manual
### 3.1 Adding a new admin
To add a new admin you need to add his telegram id to the ADMIN_ID_LIST environment variable, separated by commas, and reload the bot.<br>For example: ``ADMIN_ID_LIST=123456,654321``
### 3.2 Send to all bot users functionality
* Open the admin panel by entering the command ``/admin``, then click on the "Send to everyone" button.
* Type a message or forward to the bot, the bot supports sending a message with pictures and Telegram markup (bold, italics, spoilers, etc.).
* Confirm or decline the sending of messages.
* After successful message sending, the original message with inline buttons "Confirm", "Decline" will change to ``Message sent to x out of y people``.
### 3.3 Adding new items to the bot
>**Note**
> The functionality for adding new products so far supports adding products using .json files.
* Open the admin panel by entering the command ``/admin``.
* Click on the "Add new items" button.
* Send .json file with new items.<br>Example of .json file:
```
{
	"items": [
		{
			"category": "Category#1",
			"subcategory": "Subcategory#1",
			"price": 50,
			"description": "Mocked description",
			"private_data": "Mocked private data"
		},
		{
			"category": "Category#2",
			"subcategory": "Subcategory#2",
			"price": 100,
			"description": "Mocked description",
			"private_data": "Mocked private data"
		}
	]
}
```
>**Note**
> The "private_data" property is what the user gets when they make a purchase.
### 3.4 Send to everyone restocking message
* Open the admin panel by entering the command ``/admin``.
* Click on the "Send restocking message" button.
>**Note**
> Restocking message is generated automatically and looks as follows:
```
📅 Update YYYY-MM-DD

📁 Category Category#1

📄 Subcategory Subcategory#1 1 pcs

📁 Category Category#2

📄 Subcategory Subcategory#2 1 pcs
``` 
### 3.5 Get new users
* Open the admin panel by entering the command ``/admin``.
* Click on the "Send restocking message" button.
* You will get a message ``x new users:`` with inline buttons that will take you to chat with each new user who has a nickname.
### 3.6 Delete categories and subcategories
* Open the admin panel by entering the command ``/admin``.
* Click on the ``Delete category`` or ``Delete subcategory`` button.
* Select a category or subcategory, confirm deletion. If the deletion is successful, you will receive the message ``Sucessfully deleted {name} category/subcategory``
### 3.7 Make refund
>**Note**
> Refunds returns money to the user's balance in the bot.
* Open the admin panel by entering the command ``/admin``.
* Click on the ``Make refund`` button.
* In the received message you will have inline buttons in the format ``TelegramID/TelegramUsername|TotalPrice|SubcategoryName``.<br>For example: ``@Paul5599|$100.0|Test subcategory``.
* Select the purchase for which you want to make a refund.
* You will receive a message in ``Do you really want to refund user @durov for purchasing 1 SubcategoryName in the amount of $100.0`` format. Confirm or decline the refund.
* If the refund is successful, you will receive a ``Successfully refunded $100.0 to user Paul5599 for purchasing 1 SubcategoryName`` message.
* The user to whom you returned the message will also receive a message in ``You have been refunded $20.0 for the purchase of 1 pieces of SubcategoryName`` format.
### 3.8 Admin notifications
>**Note**
> All users with telegram id in the .env ADMIN_ID_LIST environment variable will receive these notifications
#### 3.8.1 Notification to admin about new deposit
* If any user topped up the balance and clicked on the "Refresh balance" button, you will receive the following message from the bot:
```
New deposit by user with username @Paul1565 for $100 with 100USDT
USDT address:TCq7aJKnGYXomuABUhqSBQwnmKJNzeZpJr
Seed: abandon abandon abandon abandon abandon abandon abandon abandon abandon abandon abandon abandon
```
#### 3.8.2 Notification to admin about new buy
After each purchase, you will receive a message in the format:
```
A new purchase by user @Paul5599 for the amount of $100 for the purchase of a 1 pcs SubcategoryName.
```
