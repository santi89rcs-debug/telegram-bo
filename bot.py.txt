import os
from telegram import Bot
import asyncio
from datetime import datetime
import pytz

TOKEN = os.getenv("TOKEN")
CHAT_ID = int(os.getenv("CHAT_ID"))

bot = Bot(token=TOKEN)

TIMEZONE = pytz.timezone("Europe/Warsaw")

async def send_poll():
    await bot.send_poll(
        chat_id=CHAT_ID,
        question="Тренировка сегодня в 18:30",
        options=["Приду", "Не приду"],
        is_anonymous=False
    )

async def scheduler():
    while True:
        now = datetime.now(TIMEZONE)

        if now.weekday() in [1, 3]:  # вторник и четверг
            if now.hour == 11 and now.minute == 0:
                await send_poll()
                await asyncio.sleep(60)

        await asyncio.sleep(20)

asyncio.run(scheduler())