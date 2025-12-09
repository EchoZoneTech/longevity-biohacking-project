# AI-Powered Video Creation with Google Cloud

This guide shows you how to use Google Cloud AI services to automate video creation and text-to-speech for your @EchoZone-Health YouTube videos.

## 🤖 Overview: AI-Automated Video Production

Instead of manually recording voiceovers and editing, you can use AI services to:
- **Convert script text to natural speech** (Google Cloud Text-to-Speech)
- **Generate videos with AI** (Google Cloud Video AI + third-party services)
- **Add subtitles automatically** (Google Cloud Speech-to-Text)
- **Automate editing workflows** (Google Cloud APIs)

---

## 🎙️ Google Cloud Text-to-Speech (TTS)

**What it does:** Converts your script text into natural-sounding AI voice narration.

### Service Link
**Google Cloud Text-to-Speech:** https://cloud.google.com/text-to-speech

### Setup Steps

1. **Create Google Cloud Account**
   - Go to: https://console.cloud.google.com/
   - Sign up (get $300 free credit for new users)

2. **Enable Text-to-Speech API**
   - Navigate to: https://console.cloud.google.com/apis/library/texttospeech.googleapis.com
   - Click "Enable"

3. **Install Google Cloud SDK** (optional, for command-line usage)
   - Download: https://cloud.google.com/sdk/docs/install

### Pricing
- **First 1 million characters/month:** FREE (Standard voices)
- **WaveNet voices:** $16 per 1 million characters
- **Neural2 voices:** $16 per 1 million characters (most natural)

### Recommended Voices for Health Content

**For Professional Male Voice:**
- `en-US-Neural2-D` (Deep, authoritative)
- `en-US-Neural2-A` (Clear, professional)
- `en-US-Studio-M` (Premium quality, most expensive)

**For Professional Female Voice:**
- `en-US-Neural2-F` (Warm, engaging)
- `en-US-Neural2-C` (Clear, professional)
- `en-US-Studio-O` (Premium quality)

### Using Text-to-Speech API

**Option 1: Web Interface (Easiest)**
- Go to: https://cloud.google.com/text-to-speech#section-2
- Paste your script text
- Select voice (recommend Neural2 or Studio)
- Download MP3 file

**Option 2: Python Script** (Recommended for bulk processing)
```python
# Install: pip install google-cloud-texttospeech

from google.cloud import texttospeech

# Initialize client
client = texttospeech.TextToSpeechClient()

# Read your script
with open('scripts/youtube/longevity_biohacking_intro.md', 'r') as f:
    script_text = f.read()

# Configure voice
voice = texttospeech.VoiceSelectionParams(
    language_code="en-US",
    name="en-US-Neural2-D",  # Professional male voice
    ssml_gender=texttospeech.SsmlVoiceGender.MALE
)

# Configure audio
audio_config = texttospeech.AudioConfig(
    audio_encoding=texttospeech.AudioEncoding.MP3,
    speaking_rate=1.0,  # Adjust speed (0.25 to 4.0)
    pitch=0.0  # Adjust pitch (-20 to 20)
)

# Generate speech
synthesis_input = texttospeech.SynthesisInput(text=script_text)
response = client.synthesize_speech(
    input=synthesis_input,
    voice=voice,
    audio_config=audio_config
)

# Save audio file
with open('voiceover.mp3', 'wb') as out:
    out.write(response.audio_content)
    print('Audio saved to voiceover.mp3')
```

**Option 3: REST API**
```bash
curl -X POST \
  -H "Authorization: Bearer $(gcloud auth print-access-token)" \
  -H "Content-Type: application/json; charset=utf-8" \
  -d @request.json \
  "https://texttospeech.googleapis.com/v1/text:synthesize"
```

### Tips for Better TTS Quality

1. **Use SSML** (Speech Synthesis Markup Language) for better control:
```xml
<speak>
  <prosody rate="slow" pitch="-2st">
    Welcome to Longevity Biohacking.
  </prosody>
  <break time="500ms"/>
  Today we're exploring five powerful biohacks...
</speak>
```

2. **Break script into sections** for varied pacing
3. **Add pauses** between sections using `<break time="1s"/>`
4. **Emphasize key words** using `<emphasis level="strong">autophagy</emphasis>`

---

## 🎬 AI Video Generation Services

Google Cloud doesn't have a direct "text-to-video" service, but you can combine services:

### Option 1: Google Cloud Video Intelligence API
**Link:** https://cloud.google.com/video-intelligence

**What it does:**
- Analyzes and tags video content (not generation)
- Useful for: Auto-generating video tags, detecting scenes
- **Not recommended for creating videos from scratch**

### Option 2: Third-Party AI Video Services (Better Options)

**Pictory.ai** (Recommended for health content)
- Link: https://pictory.ai
- Converts scripts to videos automatically
- Integrates stock footage from libraries
- Adds AI voiceover
- Pricing: $23-47/month
- **Best for:** Automated video from text scripts

**Synthesia.io**
- Link: https://www.synthesia.io
- AI avatars present your script
- 140+ languages
- Pricing: $30/month (starter)
- **Best for:** On-camera presenter without filming

**Descript**
- Link: https://www.descript.com
- AI video editing + overdub voice cloning
- Screen recording and editing
- Pricing: Free tier, paid from $12/month
- **Best for:** Video editing with AI assistance

**Runway ML**
- Link: https://runwayml.com
- AI video generation and editing
- Text-to-video features
- Pricing: Free tier, paid from $12/month

**Elai.io**
- Link: https://elai.io
- AI video generation from text
- Multiple AI avatars
- Pricing: $29/month
- **Best for:** Educational content with AI presenter

---

## 🎨 Complete AI-Automated Workflow

### Recommended Stack: Google Cloud TTS + Pictory.ai

**Step 1: Generate Voiceover (Google Cloud TTS)**
1. Go to https://console.cloud.google.com/text-to-speech
2. Paste voiceover sections from your script
3. Select Neural2 or Studio voice
4. Download MP3 files

**Step 2: Create Video (Pictory.ai)**
1. Sign up at https://pictory.ai
2. Upload your script or paste text
3. Upload the TTS voiceover (or use Pictory's built-in TTS)
4. Pictory auto-matches stock footage to your content
5. Customize scenes, text overlays, music
6. Export video (1080p)

**Cost:** ~$40-50/month (TTS free tier + Pictory subscription)
**Time:** 2-4 hours per video (vs 20-30 hours manual)

---

## 🔧 Google Cloud Services Integration

### 1. Text-to-Speech API
**Link:** https://cloud.google.com/text-to-speech
**Use:** Generate voiceovers
**Pricing:** Free tier available

### 2. Translation API (if creating multilingual content)
**Link:** https://cloud.google.com/translate
**Use:** Translate scripts to other languages
**Pricing:** $20 per 1M characters

### 3. Natural Language API
**Link:** https://cloud.google.com/natural-language
**Use:** Extract keywords for tags, analyze sentiment
**Pricing:** Free tier available

### 4. Video Intelligence API
**Link:** https://cloud.google.com/video-intelligence
**Use:** Auto-generate tags after video creation
**Pricing:** First 1000 minutes/month free

### 5. Speech-to-Text API
**Link:** https://cloud.google.com/speech-to-text
**Use:** Generate subtitles from final video
**Pricing:** First 60 minutes/month free

---

## 💰 Cost Comparison

### Manual Production
- Time: 20-30 hours/video
- Cost: $0 (if DIY) or $200-500 (if hiring)

### Semi-Automated (TTS + Manual Editing)
- Time: 10-15 hours/video
- Cost: $0-20/month (TTS + free editing software)

### Fully Automated (TTS + AI Video Platform)
- Time: 2-4 hours/video
- Cost: $40-100/month (subscriptions)

### Best Value for @EchoZone-Health
**Recommended:** Semi-automated approach
- Google Cloud TTS for voiceover (free tier or ~$5/video)
- Manual editing with stock footage ($0 using free sources)
- Total: ~$5/video, 10-15 hours production time

---

## 📋 Step-by-Step: Using Google Cloud TTS for Your Scripts

### Quick Start Guide

**1. Set Up Google Cloud Project**
```bash
# Visit https://console.cloud.google.com/
# Create new project: "EchoZone-Health-Videos"
# Enable Text-to-Speech API
```

**2. Extract Voiceover Text from Script**
```bash
# From your script file, copy only "Voiceover:" sections
# Example from longevity_biohacking_intro.md:
# Lines starting with "**Voiceover:**" contain the text to convert
```

**3. Generate Audio (Web Interface - Easiest)**
- Go to: https://cloud.google.com/text-to-speech#section-2
- Paste voiceover text
- Choose voice: `en-US-Neural2-D` (professional male)
- Click "Speak It"
- Download MP3

**4. Generate Audio (Python - Best for Automation)**
```python
# Save this as generate_voiceover.py

from google.cloud import texttospeech
import os

def text_to_speech(text, output_file, voice_name="en-US-Neural2-D"):
    client = texttospeech.TextToSpeechClient()
    
    synthesis_input = texttospeech.SynthesisInput(text=text)
    
    voice = texttospeech.VoiceSelectionParams(
        language_code="en-US",
        name=voice_name
    )
    
    audio_config = texttospeech.AudioConfig(
        audio_encoding=texttospeech.AudioEncoding.MP3,
        speaking_rate=1.0,
        pitch=0.0
    )
    
    response = client.synthesize_speech(
        input=synthesis_input,
        voice=voice,
        audio_config=audio_config
    )
    
    with open(output_file, 'wb') as out:
        out.write(response.audio_content)
    
    print(f'Audio saved to {output_file}')

# Usage
voiceover_text = """
What if I told you that living to 100 years old - healthy, vibrant, 
and full of energy - isn't just a dream, but a scientific possibility?
"""

text_to_speech(voiceover_text, "intro_voiceover.mp3")
```

**5. Use Audio in Video Editor**
- Import MP3 into DaVinci Resolve, CapCut, or Pictory
- Add stock footage synchronized to audio
- Export final video

---

## 🚀 Automation Script for All 4 Videos

**Python script to generate all voiceovers:**

```python
# generate_all_voiceovers.py

from google.cloud import texttospeech
import re

def extract_voiceover_from_script(file_path):
    """Extract all voiceover text from a script file"""
    with open(file_path, 'r') as f:
        content = f.read()
    
    # Find all sections with **Voiceover:**
    pattern = r'\*\*Voiceover:\*\*\s*\n(.*?)(?=\n\*\*|---|\Z)'
    matches = re.findall(pattern, content, re.DOTALL)
    
    # Join all voiceover sections
    voiceover_text = ' '.join(matches)
    return voiceover_text.strip()

def generate_voiceover(script_path, output_path, voice="en-US-Neural2-D"):
    """Generate voiceover from script"""
    client = texttospeech.TextToSpeechClient()
    
    # Extract text
    text = extract_voiceover_from_script(script_path)
    
    # Configure
    synthesis_input = texttospeech.SynthesisInput(text=text)
    voice_params = texttospeech.VoiceSelectionParams(
        language_code="en-US",
        name=voice
    )
    audio_config = texttospeech.AudioConfig(
        audio_encoding=texttospeech.AudioEncoding.MP3,
        speaking_rate=1.0
    )
    
    # Generate
    response = client.synthesize_speech(
        input=synthesis_input,
        voice=voice_params,
        audio_config=audio_config
    )
    
    # Save
    with open(output_path, 'wb') as out:
        out.write(response.audio_content)
    
    print(f'Generated: {output_path}')

# Generate all 4 videos
scripts = [
    ('scripts/youtube/longevity_biohacking_intro.md', 'voiceover_1_intro.mp3'),
    ('scripts/youtube/sleep_optimization_masterclass.md', 'voiceover_2_sleep.mp3'),
    ('scripts/youtube/nad_boosting_guide.md', 'voiceover_3_nad.mp3'),
    ('scripts/youtube/advanced_fasting_protocols.md', 'voiceover_4_fasting.mp3')
]

for script_path, output_path in scripts:
    generate_voiceover(script_path, output_path)

print('All voiceovers generated!')
```

---

## 🎯 Recommended Setup for @EchoZone-Health

### Best Approach: Hybrid AI + Manual

**For Voiceovers:**
✅ Use Google Cloud Text-to-Speech (Neural2 voices)
- Natural sounding
- Cost-effective ($0-5/video)
- Full control over script

**For Visuals:**
✅ Use free stock footage (Pexels) + manual editing
- Higher quality than AI-generated
- More control over content
- One-time learning curve

**For Editing:**
✅ Use CapCut or DaVinci Resolve
- Free, powerful tools
- Better results than fully automated

**For Thumbnails:**
✅ Use Canva with AI features
- AI image generation available
- Professional templates

### Alternative: Full Automation

**If you want 100% automated:**
1. Use Pictory.ai ($47/month)
2. Upload scripts
3. Let AI generate everything
4. Review and publish

**Pros:** Minimal time (2-3 hrs/video)
**Cons:** Less control, generic look, monthly cost

---

## 📚 Additional Resources

### Google Cloud Documentation
- **TTS Quickstart:** https://cloud.google.com/text-to-speech/docs/quickstart-client-libraries
- **Voice List:** https://cloud.google.com/text-to-speech/docs/voices
- **SSML Guide:** https://cloud.google.com/text-to-speech/docs/ssml
- **Pricing Calculator:** https://cloud.google.com/products/calculator

### Video AI Platforms Comparison
- **Pictory.ai:** Best for stock footage + TTS
- **Synthesia:** Best for AI avatar presenter
- **Descript:** Best for editing with AI assist
- **Runway ML:** Best for creative AI effects

### Learning Resources
- YouTube: "Google Cloud Text-to-Speech Tutorial"
- YouTube: "Pictory AI Tutorial for Beginners"
- YouTube: "Automated YouTube Video Creation"

---

## 🎬 Your Action Plan

### This Week:
1. ✅ Sign up for Google Cloud (free $300 credit)
2. ✅ Enable Text-to-Speech API
3. ✅ Test TTS with a short script excerpt
4. ✅ Choose automation level (full vs. semi)

### Next Week:
5. ✅ Generate voiceover for Video 1 using TTS
6. ✅ If fully automated: Sign up for Pictory.ai trial
7. ✅ If semi-automated: Download stock footage from Pexels
8. ✅ Create first video

### Decision Point:
- **Full automation:** $40-100/month, 2-4 hrs/video, less control
- **Semi-automation:** $0-20/month, 10-15 hrs/video, more control
- **Manual:** $0/month, 20-30 hrs/video, full control

**Recommendation for beginners:** Start with semi-automated (TTS + manual editing) to learn the process, then consider full automation later.

---

## Summary: Google Cloud Services

**Primary Tool:**
- **Text-to-Speech:** https://cloud.google.com/text-to-speech

**Supporting Tools:**
- **Translation API:** https://cloud.google.com/translate (multilingual)
- **Natural Language API:** https://cloud.google.com/natural-language (keywords)
- **Video Intelligence:** https://cloud.google.com/video-intelligence (analysis)
- **Speech-to-Text:** https://cloud.google.com/speech-to-text (captions)

**Third-Party Integration:**
- **Pictory.ai:** Full video automation
- **Synthesia.io:** AI avatar videos
- **Descript:** AI-assisted editing

**Total estimated cost:** $0-100/month depending on automation level chosen.
