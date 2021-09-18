# notioncal
test file
{
 "cells": [
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Integrating Google Calendar API in Python Projects\n",
    "\n",
    "![](http://chittagongit.com/download/229209)\n",
    "\n",
    "- [Google Calendar](https://calendar.google.com)\n",
    "\n",
    "- [Google Calendar API](https://developers.google.com/calendar/)\n",
    "\n",
    "- [Google Developers Console](https://console.developers.google.com/)\n",
    "\n",
    "- [Google Calendar API Scopes](https://developers.google.com/calendar/auth)\n",
    "\n",
    "- [Google Calendar API Reference](https://developers.google.com/calendar/v3/reference/)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Installation\n",
    "\n",
    "```\n",
    "pip install google-api-python-client\n",
    "```"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## OAuth 2.0 Setup"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 1,
   "metadata": {},
   "outputs": [],
   "source": [
    "from apiclient.discovery import build\n",
    "from google_auth_oauthlib.flow import InstalledAppFlow"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 2,
   "metadata": {},
   "outputs": [],
   "source": [
    "scopes = ['https://www.googleapis.com/auth/calendar']"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "flow = InstalledAppFlow.from_client_secrets_file(\"client_secret.json\", scopes=scopes)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "credentials = flow.run_console()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 3,
   "metadata": {},
   "outputs": [],
   "source": [
    "import pickle"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "pickle.dump(credentials, open(\"token.pkl\", \"wb\"))"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 4,
   "metadata": {},
   "outputs": [],
   "source": [
    "credentials = pickle.load(open(\"token.pkl\", \"rb\"))"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 5,
   "metadata": {},
   "outputs": [],
   "source": [
    "service = build(\"calendar\", \"v3\", credentials=credentials)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Get My Calendars"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 6,
   "metadata": {},
   "outputs": [],
   "source": [
    "result = service.calendarList().list().execute()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 7,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "{'kind': 'calendar#calendarListEntry',\n",
       " 'etag': '\"1557422762384000\"',\n",
       " 'id': 'indianpythonista@gmail.com',\n",
       " 'summary': 'indianpythonista@gmail.com',\n",
       " 'timeZone': 'Asia/Kolkata',\n",
       " 'colorId': '14',\n",
       " 'backgroundColor': '#9fe1e7',\n",
       " 'foregroundColor': '#000000',\n",
       " 'selected': True,\n",
       " 'accessRole': 'owner',\n",
       " 'defaultReminders': [{'method': 'popup', 'minutes': 30}],\n",
       " 'notificationSettings': {'notifications': [{'type': 'eventCreation',\n",
       "    'method': 'email'},\n",
       "   {'type': 'eventChange', 'method': 'email'},\n",
       "   {'type': 'eventCancellation', 'method': 'email'},\n",
       "   {'type': 'eventResponse', 'method': 'email'}]},\n",
       " 'primary': True,\n",
       " 'conferenceProperties': {'allowedConferenceSolutionTypes': ['eventHangout']}}"
      ]
     },
     "execution_count": 7,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "result['items'][0]"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Get My Calendar Events"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 8,
   "metadata": {},
   "outputs": [],
   "source": [
    "calendar_id = result['items'][0]['id']"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 9,
   "metadata": {},
   "outputs": [],
   "source": [
    "result = service.events().list(calendarId=calendar_id, timeZone=\"Asia/Kolkata\").execute()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 10,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "{'kind': 'calendar#event',\n",
       " 'etag': '\"3114856233680000\"',\n",
       " 'id': '0slqq110a9171scjmrfojjquse',\n",
       " 'status': 'confirmed',\n",
       " 'htmlLink': 'https://www.google.com/calendar/event?eid=MHNscXExMTBhOTE3MXNjam1yZm9qanF1c2UgaW5kaWFucHl0aG9uaXN0YUBt&ctz=Asia/Kolkata',\n",
       " 'created': '2019-05-09T18:55:16.000Z',\n",
       " 'updated': '2019-05-09T18:55:16.840Z',\n",
       " 'summary': 'Meeting',\n",
       " 'creator': {'email': 'indianpythonista@gmail.com', 'self': True},\n",
       " 'organizer': {'email': 'indianpythonista@gmail.com', 'self': True},\n",
       " 'start': {'dateTime': '2019-05-05T02:30:00+05:30'},\n",
       " 'end': {'dateTime': '2019-05-05T03:30:00+05:30'},\n",
       " 'iCalUID': '0slqq110a9171scjmrfojjquse@google.com',\n",
       " 'sequence': 0,\n",
       " 'extendedProperties': {'private': {'everyoneDeclinedDismissed': '-1'}},\n",
       " 'reminders': {'useDefault': True}}"
      ]
     },
     "execution_count": 10,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "result['items'][0]"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Create a New Calandar Event"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 11,
   "metadata": {},
   "outputs": [],
   "source": [
    "from datetime import datetime, timedelta"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 12,
   "metadata": {},
   "outputs": [],
   "source": [
    "start_time = datetime(2019, 5, 12, 19, 30, 0)\n",
    "end_time = start_time + timedelta(hours=4)\n",
    "timezone = 'Asia/Kolkata'"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 13,
   "metadata": {},
   "outputs": [],
   "source": [
    "event = {\n",
    "  'summary': 'IPL Final 2019',\n",
    "  'location': 'Hyderabad',\n",
    "  'description': 'MI vs TBD',\n",
    "  'start': {\n",
    "    'dateTime': start_time.strftime(\"%Y-%m-%dT%H:%M:%S\"),\n",
    "    'timeZone': timezone,\n",
    "  },\n",
    "  'end': {\n",
    "    'dateTime': end_time.strftime(\"%Y-%m-%dT%H:%M:%S\"),\n",
    "    'timeZone': timezone,\n",
    "  },\n",
    "  'reminders': {\n",
    "    'useDefault': False,\n",
    "    'overrides': [\n",
    "      {'method': 'email', 'minutes': 24 * 60},\n",
    "      {'method': 'popup', 'minutes': 10},\n",
    "    ],\n",
    "  },\n",
    "}"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 14,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "{'kind': 'calendar#event',\n",
       " 'etag': '\"3114964833447000\"',\n",
       " 'id': 'advb4ftvjbivf7jaru9g1dbp5s',\n",
       " 'status': 'confirmed',\n",
       " 'htmlLink': 'https://www.google.com/calendar/event?eid=YWR2YjRmdHZqYml2ZjdqYXJ1OWcxZGJwNXMgaW5kaWFucHl0aG9uaXN0YUBt',\n",
       " 'created': '2019-05-10T10:00:16.000Z',\n",
       " 'updated': '2019-05-10T10:00:16.759Z',\n",
       " 'summary': 'IPL Final 2019',\n",
       " 'description': 'MI vs TBD',\n",
       " 'location': 'Hyderabad',\n",
       " 'creator': {'email': 'indianpythonista@gmail.com', 'self': True},\n",
       " 'organizer': {'email': 'indianpythonista@gmail.com', 'self': True},\n",
       " 'start': {'dateTime': '2019-05-12T19:30:00+05:30',\n",
       "  'timeZone': 'Asia/Kolkata'},\n",
       " 'end': {'dateTime': '2019-05-12T23:30:00+05:30', 'timeZone': 'Asia/Kolkata'},\n",
       " 'iCalUID': 'advb4ftvjbivf7jaru9g1dbp5s@google.com',\n",
       " 'sequence': 0,\n",
       " 'reminders': {'useDefault': False,\n",
       "  'overrides': [{'method': 'email', 'minutes': 1440},\n",
       "   {'method': 'popup', 'minutes': 10}]}}"
      ]
     },
     "execution_count": 14,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "service.events().insert(calendarId=calendar_id, body=event).execute()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Utility function\n",
    "\n",
    "```\n",
    "pip install datefinder\n",
    "```"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 15,
   "metadata": {},
   "outputs": [],
   "source": [
    "import datefinder"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 16,
   "metadata": {},
   "outputs": [],
   "source": [
    "matches = datefinder.find_dates(\"5 may 9 PM\")"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 17,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "[datetime.datetime(2019, 5, 5, 21, 0)]"
      ]
     },
     "execution_count": 17,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "list(matches)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 18,
   "metadata": {},
   "outputs": [],
   "source": [
    "def create_event(start_time_str, summary, duration=1, description=None, location=None):\n",
    "    matches = list(datefinder.find_dates(start_time_str))\n",
    "    if len(matches):\n",
    "        start_time = matches[0]\n",
    "        end_time = start_time + timedelta(hours=duration)\n",
    "    \n",
    "    event = {\n",
    "        'summary': summary,\n",
    "        'location': location,\n",
    "        'description': description,\n",
    "        'start': {\n",
    "            'dateTime': start_time.strftime(\"%Y-%m-%dT%H:%M:%S\"),\n",
    "            'timeZone': 'Asia/Kolkata',\n",
    "        },\n",
    "        'end': {\n",
    "            'dateTime': end_time.strftime(\"%Y-%m-%dT%H:%M:%S\"),\n",
    "            'timeZone': 'Asia/Kolkata',\n",
    "        },\n",
    "        'reminders': {\n",
    "            'useDefault': False,\n",
    "            'overrides': [\n",
    "                {'method': 'email', 'minutes': 24 * 60},\n",
    "                {'method': 'popup', 'minutes': 10},\n",
    "            ],\n",
    "        },\n",
    "    }\n",
    "    return service.events().insert(calendarId='primary', body=event).execute()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 19,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "{'kind': 'calendar#event',\n",
       " 'etag': '\"3114964858585000\"',\n",
       " 'id': '43rehpareejd6nm44u8upvsir8',\n",
       " 'status': 'confirmed',\n",
       " 'htmlLink': 'https://www.google.com/calendar/event?eid=NDNyZWhwYXJlZWpkNm5tNDR1OHVwdnNpcjggaW5kaWFucHl0aG9uaXN0YUBt',\n",
       " 'created': '2019-05-10T10:00:29.000Z',\n",
       " 'updated': '2019-05-10T10:00:29.322Z',\n",
       " 'summary': 'Meeting',\n",
       " 'creator': {'email': 'indianpythonista@gmail.com', 'self': True},\n",
       " 'organizer': {'email': 'indianpythonista@gmail.com', 'self': True},\n",
       " 'start': {'dateTime': '2019-05-15T21:00:00+05:30',\n",
       "  'timeZone': 'Asia/Kolkata'},\n",
       " 'end': {'dateTime': '2019-05-15T22:00:00+05:30', 'timeZone': 'Asia/Kolkata'},\n",
       " 'iCalUID': '43rehpareejd6nm44u8upvsir8@google.com',\n",
       " 'sequence': 0,\n",
       " 'reminders': {'useDefault': False,\n",
       "  'overrides': [{'method': 'email', 'minutes': 1440},\n",
       "   {'method': 'popup', 'minutes': 10}]}}"
      ]
     },
     "execution_count": 19,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "create_event(\"15 may 9 PM\", \"Meeting\")"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Python 3",
   "language": "python",
   "name": "python3"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.6.7"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 2
}
