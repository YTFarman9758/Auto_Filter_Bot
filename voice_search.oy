from pyrogram import Client, filters
import whisper
import os

model = whisper.load_model("base")

@Client.on_message(filters.private & filters.voice)
async def voice_search(client, message):

    processing = await message.reply_text(
        "🎤 Converting voice to text..."
    )

    try:
        voice_file = await message.download()

        result = model.transcribe(
            voice_file
        )

        search_text = result["text"].strip()

        await processing.edit_text(
            f"🔍 Searching:\n<code>{search_text}</code>"
        )

        # Fake text message for auto filter
        message.text = search_text

        from plugins.pmfilter import auto_filter

        await auto_filter(
            client,
            message
        )

        try:
            os.remove(voice_file)
        except:
            pass

    except Exception as e:
        await processing.edit_text(
            f"❌ Error:\n<code>{e}</code>"
        )
