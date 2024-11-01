import discord
import re  # For regex to extract numbers
from discord.ext import commands

intents = discord.Intents.default()
intents.message_content = True  # Enable reading message content
bot = commands.Bot(command_prefix="!", intents=intents)

# Dictionary to store counts for tracked items
text_counts = {}

@bot.event
async def on_ready():
    print(f"Logged in as {bot.user}")

@bot.event
async def on_message(message):
    if message.author == bot.user:
        return  # Ignore messages from the bot itself

    # Normalize the message to lowercase
    normalized_content = message.content.lower()

    # Increment the count for user messages
    if normalized_content in text_counts:
        text_counts[normalized_content] += 1
    else:
        text_counts[normalized_content] = 1

    # Check if the message is from a webhook
    if message.webhook_id:
        # Check embedded content
        for embed in message.embeds:
            # Extract number for "Green Essence Stone"
            if embed.description and "green essence stone" in embed.description.lower():
                # Use regex to find the number
                match = re.search(r"green essence stone\s*:\s*x?(\d+)", embed.description.lower())
                if match:
                    quantity = int(match.group(1))  # Convert matched string to integer
                    if "green essence stone" in text_counts:
                        text_counts["green essence stone"] += quantity
                    else:
                        text_counts["green essence stone"] = quantity

            # Check fields in embeds as well
            for field in embed.fields:
                if "green essence stone" in field.value.lower():
                    # Use regex to find the number
                    match = re.search(r"green essence stone\s*:\s*x?(\d+)", field.value.lower())
                    if match:
                        quantity = int(match.group(1))  # Convert matched string to integer
                        if "green essence stone" in text_counts:
                            text_counts["green essence stone"] += quantity
                        else:
                            text_counts["green essence stone"] = quantity

    # Check if the message starts with "?" to query counts
    if normalized_content.startswith("?"):
        query_word = normalized_content[1:]  # Get the word after "?"
        if query_word == "green":
            total_count = text_counts.get("green essence stone", 0)
            await message.channel.send(f"You get Green Essence Stone: x{total_count}.")

    await bot.process_commands(message)

# Run the bot with your token
bot.run(bot_token)
