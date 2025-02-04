import logging
import requests
from aiogram import Bot, Dispatcher, types
from aiogram.utils import executor

# Токен Telegram бота
TOKEN = "7889892017:AAENdUzgFZ6jUXRx3M4Zy1e8JvNKMcMqzM8"
bot = Bot(token=TOKEN)
dp = Dispatcher(bot)

# Ваш партнёрский код Amazon
AMAZON_PARTNER_CODE = "AIMPVIH0W1NSI"

# Настройка логирования
logging.basicConfig(level=logging.INFO)

@dp.message_handler(commands=['start', 'help'])
async def send_welcome(message: types.Message):
    text = ("Привет! Я бот для поиска товаров на Amazon.\n"
            "Отправь мне название товара, и я найду его для тебя!")
    await message.reply(text)

@dp.message_handler()
async def search_amazon(message: types.Message):
    query = message.text.strip()
    if not query:
        await message.reply("Введите название товара для поиска.")
        return

    search_url = f"https://www.amazon.de/s?k={query.replace(' ', '+')}&tag={AMAZON_PARTNER_CODE}"
    text = f"Вот товары, которые я нашёл по запросу '{query}':\n{search_url}"
    
    await message.reply(text)

if __name__ == "__main__":
    executor.start_polling(dp, skip_updates=True)