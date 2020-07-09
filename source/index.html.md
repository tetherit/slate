---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  # - ruby
  # - python
  # - javascript

# toc_footers:
#   - <a href='#'>Sign Up for a Developer Key</a>
#   - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

# includes:
#   - errors

search: true

code_clipboard: true
---

# Introduction

Welcome to the Tether API! You can use our API to access Tether API endpoints, which can get information about events, zones, recordings and other data from our database.

You can view code examples in the dark area to the right






# oAuth2 Integration
Before you'll start work with API you need to generate your oAuth2 token.


## Register your client app

<aside class="notice">
Make sure the user is an <b>integrator</b>, you can check by going to <b>Admin -> Users -> User</b>
</aside>

Give the user permission to create an oAuth2 application by going to: <b>Admin -> Users -> User -> Permissions</b> and ticking the <b>Allow Creating oAuth2 Applications</b> checkbox:

<img src="images/access_checkbox.png" />

Create a new oauth application by going to <b>Admin -> Users -> User -> API Details</b> and clicking the <b>Create oAuth Application</b> button. Enter a valid application name and redirect uri, after saving you will see your all registered applications:

<img src="images/create_app_btn.png" width="49%" />
<img src="images/create_modal.png" width="49%"/>

<img src="images/app_list.png" width="100%"/>

## Request authorisation
To request the authorisation token, you can use a compatible oauth2 library, for example: [oauth2](https://github.com/oauth-xx/oauth2) for ruby, or by visiting the URL manually and clicking "Authorise".

`https://my.timeline.is/oauth/authorize?client_id=YOUR_API_KEY&redirect_uri=https%3A%2F%2Fmy.site.com%2Fcallback%2Furl&response_type=code`

<aside class="notice">
<b>Note:</b> Replace YOUR_API_KEY with Tether oauth Api Key
</aside>

<aside class="notice">
<b>Note:</b> If you are not logged in, you will be redirected to the login page. After successfully logging in, you will be redirected to the authorisation page.
</aside>
<img src="images/authorise_page.png" width="100%"/>

## Redirect URI
After authorisation, the system will automatically redirect you to your REDIRECT_URI, which you configured above. For example:

`https://my.site.com/callback/url?code=380762fb28c7c17d096a06fce974535c8129ab8a13fab5608a4e1c1075bbf01a`

## Requesting the Access Token
To request the access token, you need to send a post request with the returned code above. To do this, you can use any HTTP client.

> In this example, we use `rest-client` for ruby:

```shell
parameters = 'client_id=YOUR_API_KEY&client_secret=YOUR_API_SECRET&code=RETURNED_CODE&grant_type=authorization_code&redirect_uri=https%3A%2F%2Fmy.site.com%2Fcallback%2Furl'

RestClient.post 'http://my.timeline.is/oauth/token', parameters
```

> The response look something like this:

```json
{
  "access_token": "de6780bc506a0446309bd9362820ba8aed28aa506c71eedbe1c5c4f9dd350e54",
  "token_type": "Bearer",
  "expires_in": 2629746,
  "refresh_token": "8257e65c97202ed1726cf9571600918f3bffb2544b26e00a61df9897668c33a1",
  "created_at": 1569313222
}
```

# Events
## Get events in period of time

```shell
curl "https://my.timeline.is/api/v3p/events?from=2018-07-09%2019:00:00&to=2018-07-09%2020:30:00&zone_id=5d1f0bed41092775efbddc36" \
     -H "Authorization: Bearer YOUR_OAUTH_TOKEN"
```

> The above command returns JSON structured like this:

```json
{
  "data":  [
    {
      "id": "5f06c1a541092788f683f3c8",
      "type": "events",
      "attributes":  {
        "items_count": 0,
        "duration": 2520,
        "motion": 0.0,
        "filesize": 0,
        "severity": "normal",
        "starts_at": "2020-06-12T16:03:09.019+01:00",
        "ends_at": "2020-06-12T16:45:09.019+01:00",
        "has_pir": false,
        "has_lpr": false,
        "has_motion": false,
        "has_video_loss": false,
        "has_video_blind": false,
        "has_video_defocus": false,
        "has_abnormal_audio": false,
        "has_loitering": false,
        "has_people_gathering": false,
        "has_fast_moving": false,
        "has_storage_failure": false,
        "has_storage_low_space": false,
        "has_temperature": false,
        "has_storage_not_exist": false,
        "has_alarm": false,
        "has_face": false,
        "has_tripwire": false,
        "has_intrusion": false,
        "has_parking": false,
        "has_car": false,
        "has_object_removal": false,
        "has_abandoned_object": false,
        "has_missing_object": false,
        "has_scene_change": false,
        "has_tma": false,
        "complete": true,
        "flagged": true,
        "has_auto_register": false,
        "has_ip_conflict": false,
        "has_network_change": false,
        "shared": false,
        "cloud": false,
        "starts_at_seconds_since_midnight": 57789,
        "ends_at_seconds_since_midnight": 60309,
        "updated_at": "2020-07-09T08:05:09.036+01:00",
        "created_at": "2020-07-09T08:05:09.036+01:00",
        "timeline_id": "5f06c1a441092788f683f3ba",
        "zone_id": "5f06c1a541092788f683f3c7",
        "location_id": "5f06c1a441092788f683f3c6",
        "best_id": "8fd1123a441092788f683f3c6"
      }
    }
  ], "jsonapi": {"version": "1.0"}
}
```

This endpoint retrieves events in period of time between `from` and `to` dates.

### HTTP Request
`GET https://my.timeline.is/api/v3p/events`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| zone_id | query | Zone ID to find event in | yes | String |
| from | query | Timestamp of period when event was started | No | Datetime |
| to | query | Timestamp of period when event was ended | No | Datetime |


## Get event in a point of time


```shell
curl "https://my.timeline.is/api/v3p/events?at=2018-07-09%2019:00:00&zone_id=5d1f0bed41092775efbddc36" -H "Authorization: Bearer YOUR_OAUTH_TOKEN"
```

> The above command returns JSON structured like this:

```json
{
  "data":  [
    {
      "id": "5f06c1a541092788f683f3c8",
      "type": "events",
      "attributes":  {
        "items_count": 0,
        "duration": 2520,
        "motion": 0.0,
        "filesize": 0,
        "severity": "normal",
        "starts_at": "2020-06-12T16:03:09.019+01:00",
        "ends_at": "2020-06-12T16:45:09.019+01:00",
        "has_pir": false,
        "has_lpr": false,
        "has_motion": false,
        "has_video_loss": false,
        "has_video_blind": false,
        "has_video_defocus": false,
        "has_abnormal_audio": false,
        "has_loitering": false,
        "has_people_gathering": false,
        "has_fast_moving": false,
        "has_storage_failure": false,
        "has_storage_low_space": false,
        "has_temperature": false,
        "has_storage_not_exist": false,
        "has_alarm": false,
        "has_face": false,
        "has_tripwire": false,
        "has_intrusion": false,
        "has_parking": false,
        "has_car": false,
        "has_object_removal": false,
        "has_abandoned_object": false,
        "has_missing_object": false,
        "has_scene_change": false,
        "has_tma": false,
        "complete": true,
        "flagged": true,
        "has_auto_register": false,
        "has_ip_conflict": false,
        "has_network_change": false,
        "shared": false,
        "cloud": false,
        "starts_at_seconds_since_midnight": 57789,
        "ends_at_seconds_since_midnight": 60309,
        "updated_at": "2020-07-09T08:05:09.036+01:00",
        "created_at": "2020-07-09T08:05:09.036+01:00",
        "timeline_id": "5f06c1a441092788f683f3ba",
        "zone_id": "5f06c1a541092788f683f3c7",
        "location_id": "5f06c1a441092788f683f3c6",
        "best_id": "8fd1123a441092788f683f3c6"
      }
    }
  ], "jsonapi": {"version": "1.0"}
}
```

This endpoint retrieves single event in a point of time.

### HTTP Request
`GET https://my.timeline.is/api/v3p/events`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| zone_id | query | Zone ID to find event in | yes | String |
| at | query | Timestamp of event to search | yes | Datetime |




## Filter events on Web site

You can generate correct url to our site with filters and `return_url`. We display banner at the top of site with link to your `redirect_url`.
It's very useful if you want to display events on our site and have possibility to return to yours

If you want to display events in time period use `events[from_time]` and `events[to_time]` parameters. In this case `timeline_id` is required parameter. If you don't pass correct `timeline_id`, our site will display events for selected timeline on the site.

If you want to get event at a certain point in time - use `at` parameter. In this case `zone` is required parameter.

<aside class="notice">
Note: if you'll not pass `return_url` in parameters we'll not display banner at the top of site
</aside>

### HTTP Request
`https://my.timeline.is/events`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| at | query | Timestamp of event to search | No |  |
| zone | query | Zone ID to find event in. Is required if you pass `at` parameter | Yes | String |
| events[from_time] | query | Parameter for filter. Uses to filter by time start. Ex: `20:30` | No | String |
| events[to_time] | query | Parameter for filter. Uses to filter by time start as end value. Ex: `21:00` | No | String |
| events[date] | query | Parameter for filter. Uses to filter by date. Ex: `2020-06-21` | No | String |
| timeline_id | query | ID of timeline. Is required if you what to filter events. Ex: `5cac594b8ef0f82370f18d98` | Yes | String |
| return_url | query | URL for ability to return from results page | No | String |



```shell
"https://my.timeline.is/events?events%5Bdate%5D=2020-07-21&events%5Bfrom_time%5D=20%3A30&events%5Bto_time%5D=21%3A00&return_url=http%3A%2F%2Fhost.some%2Freturn%2Furl&timeline_id=5f06cfe74109278f7ef8dab2&zone=5f06cfeb4109278f7ef8dac7"
```





# LPR
## Get license plates
This endpoint retrieves license plates between `start` and `end` dates.
### HTTP Request
`GET https://my.timeline.is/api/v3p/lpr`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| starts_at | query | Start timestamp of LPR search | No | Datetime  |
| ends_at | query | End timestamp of LPR search | No | Datetime |
| zone_id | query | Zone ID to find LPR in | yes | String |


```shell
curl "https://my.timeline.is/api/v3p/lpr?token=052om1JmDLxIOnwRLc6ppw&zone_id=5f06d0ca4109279015140bed&starts_at=2020-03-05+08%3A51%3A46.895+UTC&ends_at=2020-05-18+08%3A37%3A46.897+UTC" -H "Authorization: Bearer YOUR_OAUTH_TOKEN"
```

> The above command returns JSON structured like this:

```json
{
  "data":[
    {
      "id":"5f06d0ca4109279015140bf4",
      "type":"lprs",
      "attributes": {
        "flagged":false,
        "has_picture":false,
        "starts_at":"2020-03-05T08:51:46.895+00:00",
        "plate_number":"SV05OSL4",
        "country":"TUR",
        "starts_at_seconds_since_midnight":31906,
        "updated_at":"2020-07-09T09:09:46.896+01:00",
        "created_at":"2020-07-09T09:09:46.896+01:00",
        "timeline_id":"5f06d0ca4109279015140be7",
        "event_id":"5f06d0ca4109279015140bef",
        "camera_id":"5f06d0ca4109279015140bec",
        "agent_id":"5f06d0ca4109279015140be8",
        "zone_id":"5f06d0ca4109279015140bed",
        "location_id":"5f06d0ca4109279015140be9"
      }
    },
    {
      "id":"5f06d0ca4109279015140bf2",
      "type":"lprs",
      "attributes": {
        "flagged":false,
        "has_picture":false,
        "starts_at":"2020-04-28T01:04:46.889+01:00",
        "plate_number":"OW16DYA2",
        "country":"GBR",
        "starts_at_seconds_since_midnight":3886,
        "updated_at":"2020-07-09T09:09:46.890+01:00",
        "created_at":"2020-07-09T09:09:46.890+01:00",
        "timeline_id":"5f06d0ca4109279015140be7",
        "event_id":"5f06d0ca4109279015140bef",
        "camera_id":"5f06d0ca4109279015140bec",
        "agent_id":"5f06d0ca4109279015140be8",
        "zone_id":"5f06d0ca4109279015140bed",
        "location_id":"5f06d0ca4109279015140be9"
      }
    }
  ],
  "jsonapi": { "version": "1.0" }
}
```




# Perspectives

## Get all presets

This endpoint retrieves all presets related to user.

### HTTP Request
`GET https://my.timeline.is/api/v3p/perspectives`


```shell
curl "https://my.timeline.is/api/v3p/perspectives" -H "Authorization: Bearer YOUR_OAUTH_TOKEN"
```

> The above command returns JSON structured like this:

```json
{
  "data": [
    {
      "id": "5f06d2c541092790de7656e0",
      "type": "perspectives",
      "attributes": {
        "layout": "layout_01",
        "zone_ids": [],
        "name": "quae",
        "timeline_id": "5f06d2c441092790de7656d4",
        "user_id": "5f06d2c541092790de7656df"
      }
    }
  ],
  "jsonapi": { "version": "1.0" }
}
```



## Live switch preset

When you open our Web site and go to liveview, you can change preset by this endpoint. You need to pass `:id` of needed preset to the url.

### HTTP Request
`POST https://my.timeline.is/api/v3p/perspectives/:id/live_switch_preset`

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | No Content |








# Zones

## Get all zones

This endpoint retrieves all zones related to user.

### HTTP Request
`GET https://my.timeline.is/api/v3p/zones`


**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| location_id | query | Filters By Location Id | No | String |


```shell
curl "https://my.timeline.is/api/v3p/zones" -H "Authorization: Bearer YOUR_OAUTH_TOKEN"
```
> The above command returns JSON structured like this:

```json
{
  "data": [
    {
      "id": "5f06d4064109279130d5d6f3",
      "type": "zones",
      "attributes": {
        "name": "Camera 2"
      }
    }
  ],
  "jsonapi": { "version": "1.0" }
}
```





# Recordings

## Get list of recordings

This endpoint retrieves all recordings related to user.

### HTTP Request
`GET https://my.timeline.is/api/v3p/recordings`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| at | query | Timestamp of recording to search | No | Datetime |
| zone_id | query | Zone ID to find recording in | Yes | String |


```shell
curl "https://my.timeline.is/api/v3p/recordings?zone_id=5f06e4e741092794a7010c29" -H "Authorization: Bearer YOUR_OAUTH_TOKEN"
```
> The above command returns JSON structured like this:

```json
{
  "data": [{
    "id": "5f06e4e741092794a7010c2c",
    "type": "recordings",
    "attributes": {
      "agent_image": true,
      "agent_video": true,
      "checksum": null,
      "cloud_path": null,
      "cloud_snapshot": false,
      "cloud_thumbnail": false,
      "cloud_timelapse": false,
      "cloud_timelapse_error": null,
      "cloud_timelapse_interval": null,
      "cloud_video": false,
      "cloud_waveform": false,
      "created_at": "2020-07-09T10:35:36.096+01:00",
      "ends_at": "2020-04-25T08:03:36.094+01:00",
      "ends_at_seconds_since_midnight": 29016,
      "filesize": 0,
      "flagged": true,
      "motion": 0.0,
      "object_count": 0,
      "path": "/recordings/exercitationem1",
      "skip_notify": false,
      "starts_at": "2020-04-25T08:02:36.094+01:00",
      "starts_at_seconds_since_midnight": 28956,
      "updated_at": "2020-07-09T10:35:36.096+01:00",
      "seek_to_event": 0,
      "epoch_time": "25042020-0802AM-0803AM",
      "agent_id": "5f06e4e741092794a7010c2d",
      "camera_id": "5f06e4e741092794a7010c31",
      "device_id": null,
      "event_id": "5f06e4e741092794a7010c2a",
      "flagged_by_id": null,
      "location_id": "5f06e4e741092794a7010c28",
      "timeline_id": "5f06e4e741092794a7010c27",
      "unflagged_by_id": null,
      "zone_id": "5f06e4e741092794a7010c29",
      "urls": {
        "proxy": "/recordings/5f06e4e741092794a7010c2c/view?token=4i6yNAEWG7BWHs7FaTnPVA",
        "proxy_download": "/recordings/5f06e4e741092794a7010c2c/download",
        "proxy_snapshot": "/recordings/5f06e4e741092794a7010c2c/snapshot",
        "local": "lan-molestiae1.timeline.is/api/v3/recordings/download.mp4?recording%5Bpath%5D=%2Frecordings%2Fexercitationem1&token=vrB7WxtHEHNY1kb7XocX_A",
        "local_snapshot": "lan-molestiae1.timeline.is/api/v3/recordings/snapshot?recording%5Bpath%5D=%2Frecordings%2Fexercitationem1.jpg&token=vrB7WxtHEHNY1kb7XocX_A",
        "external": "external-molestiae1.timeline.is/api/v3/recordings/download.mp4?recording%5Bpath%5D=%2Frecordings%2Fexercitationem1&token=vrB7WxtHEHNY1kb7XocX_A",
        "external_snapshot": "external-molestiae1.timeline.is/api/v3/recordings/snapshot?recording%5Bpath%5D=%2Frecordings%2Fexercitationem1.jpg&token=vrB7WxtHEHNY1kb7XocX_A"
      },
      "prev_recording_id": null,
      "next_recording_id": null
    },
    "relationships": {
      "timelapses": {
        "meta": {
          "included": false
        }
      }
    }
  }]
}
```



## Get recording

This endpoint retrives single recording object by passed `id`

### HTTP Request
`GET https://my.timeline.is/api/v3p/recordings/:id`


```shell
curl "https://my.timeline.is/api/v3p/recordings/5f06e5f241092794fc255ded" -H "Authorization: Bearer YOUR_OAUTH_TOKEN"
```
> The above command returns JSON structured like this:

```json
{
  "data": {
    "id": "5f06e5f241092794fc255ded",
    "type": "recordings",
    "attributes": {
      "agent_image": true,
      "agent_video": true,
      "checksum": null,
      "cloud_path": null,
      "cloud_snapshot": false,
      "cloud_thumbnail": false,
      "cloud_timelapse": false,
      "cloud_timelapse_error": null,
      "cloud_timelapse_interval": null,
      "cloud_video": false,
      "cloud_waveform": false,
      "created_at": "2020-07-09T10:40:02.469+01:00",
      "ends_at": "2020-07-04T15:37:02.467+01:00",
      "ends_at_seconds_since_midnight": 56222,
      "filesize": 0,
      "flagged": false,
      "motion": 0,
      "object_count": 0,
      "path": "/recordings/officiis1",
      "skip_notify": false,
      "starts_at": "2020-07-04T15:36:02.467+01:00",
      "starts_at_seconds_since_midnight": 56162,
      "updated_at": "2020-07-09T10:40:02.469+01:00",
      "seek_to_event": 0,
      "epoch_time": "04072020-0336PM-0337PM",
      "agent_id": "5f06e5f241092794fc255dee",
      "camera_id": "5f06e5f241092794fc255df2",
      "device_id": null,
      "event_id": "5f06e5f241092794fc255deb",
      "flagged_by_id": null,
      "location_id": "5f06e5f241092794fc255de9",
      "timeline_id": "5f06e5f141092794fc255ddb",
      "unflagged_by_id": null,
      "zone_id": "5f06e5f241092794fc255de8",
      "urls": {
        "proxy": "/recordings/5f06e5f241092794fc255ded/view?token=U-Gxk1fXchjRIp2D5GDCcw",
        "proxy_download": "/recordings/5f06e5f241092794fc255ded/download",
        "proxy_snapshot": "/recordings/5f06e5f241092794fc255ded/snapshot",
        "local": "lan-facilis1.timeline.is/api/v2/recordings/download.mp4?recording%5Bpath%5D=%2Frecordings%2Fofficiis1",
        "local_snapshot": "lan-facilis1.timeline.is/api/v2/recordings/snapshot?recording%5Bpath%5D=%2Frecordings%2Fofficiis1.jpg",
        "external": "external-facilis1.timeline.is/api/v2/recordings/download.mp4?recording%5Bpath%5D=%2Frecordings%2Fofficiis1",
        "external_snapshot": "external-facilis1.timeline.is/api/v2/recordings/snapshot?recording%5Bpath%5D=%2Frecordings%2Fofficiis1.jpg"
      },
      "prev_recording_id": null,
      "next_recording_id": null
    },
    "relationships": {
      "timelapses": {
        "data": [
          {
            "type": "timelapses",
            "id": "70240479762520"
          },
          {
            "type": "timelapses",
            "id": "70240479762360"
          }
        ]
      }
    }
  },
  "included": [
    {
      "id": "70240479762520",
      "type": "timelapses",
      "attributes": {
        "id": 70240479762520,
        "src": "/api/v3p/timelapse/5f06e5f241092794fc255de8/2020-07-04%2015:35:45%20+0100/thumbnail"
      }
    },
    {
      "id": "70240479762360",
      "type": "timelapses",
      "attributes": {
        "id": 70240479762360,
        "src": "/api/v3p/timelapse/5f06e5f241092794fc255de8/2020-07-04%2015:35:50%20+0100/thumbnail"
      }
    }
  ]
}
```




## Recording on Web site

You can generate correct url to our site to display recording at a certain point in time and pass `return_url`. We display banner at the top of site with link to your `redirect_url`.
It's very useful if you want to display recording on our site and have possibility to return to yours.

<aside class="notice">
Note: if you'll not pass `return_url` in parameters we'll not display banner at the top of site
</aside>

```shell
"https://my.timeline.is/recordings?at=2018-07-09%2019:00:00&zone=5f06cfeb4109278f7ef8dac7&return_url=http%3A%2F%2Fhost.some%2Freturn%2Furl"
```

### HTTP Request
`https://my.timeline.is/recordings`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| at | query | Timestamp of recording to search | Yes | String |
| zone | query | Zone ID to find recording in | Yes | String |
| return_url | query | URL for ability to return from results page | No | String |