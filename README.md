# ElevenLabs → n8n → Google Calendar Automation
<img width="716" height="588" alt="image" src="https://github.com/user-attachments/assets/95b90946-cef5-49ab-9945-022383aef289" />

This project demonstrates a complete automation pipeline that connects an ElevenLabs agent to Google Calendar using n8n.
The system receives structured appointment data from ElevenLabs, normalizes dates and durations inside n8n, and creates calendar events reliably.

##Overview
###Goal
Automatically create Google Calendar events from conversational AI output.

###Flow

ElevenLabs agent sends structured JSON to an n8n webhook

n8n:

Normalizes date and time

Converts duration to minutes

Computes event end time

Cleans timezone / formatting issues

Google Calendar event is created with correct:

Title
Start / End time
Location
Architecture
ElevenLabs Agent
      ↓
 HTTPS Webhook (ngrok)
      ↓
     n8n
      ├─ Webhook node
      ├─ Date & Time normalization
      ├─ Duration handling
      ├─ Data cleanup
      ↓
Google Calendar API

Input Format (from ElevenLabs)

Example payload sent to n8n:

{
  "Name": "Hairdresser appointment",
  "Date": "2026/01/11",
  "Hours": "09h00",
  "Duration": "30 minutes",
  "Location": "At home"
}

Data Processing in n8n

Inside n8n, the workflow:

Converts date + time into ISO format

Converts duration into minutes

Adds duration to compute end time

Removes:

timezone offsets

milliseconds

hidden characters (\n)

Produces clean values such as:

{
  "Start": "2026-01-11T09:00:00",
  "End": "2026-01-11T09:30:00"
}


This format is fully compatible with the Google Calendar API.

Google Calendar Mapping
Google Calendar Field	Source
Summary (Title)	Name
Start	Start
End	End
Location	Location
Reminders	Default

Requirements
n8n (self-hosted via Docker)
Google Cloud project
Google Calendar API enabled
OAuth 2.0 credentials configured

ElevenLabs agent

ngrok (to expose local webhook over HTTPS)
