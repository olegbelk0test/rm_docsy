---
tags: [operators, developers, web store]
sidebar_position: 50
sidebar_label: Banners
sidebar_class_name: green
---

# Banners Onboarding Guide

The **Banners** feature provides a great opportunity to keep users visiting Web Store every day for a daily gift and making purchases to complete a stamp card and get a prize!

But it's not the end&mdash;you can set up any marketing campaigns with banners on your Web Store via a Journey flow.

## Daily Gifts User Flow

| User opens Web Store and sees a banner | User clicks on the banner and is redirected to the game to get a prize | User goes back to Web Store and sees a banner that the prize is claimed |
|-|-|-|
| ![banners_onb1.png](/img/webstore/banners_onb1.png) | ![banners_onb2.png](/img/webstore/banners_onb2.png) | ![banners_onb3.png](/img/webstore/banners_onb3.png) |

## Integration

The Web Store and Game communicate through Kafka events to manage banner flows. Typically, a Journey flow handles banner management for the Web Store. However, if the Journey is not onboarded for the Game, it can implement a custom solution using Web Store events.

### Web Store Banner Events

<table>
  <thead>
    <tr>
      <th>Web Store Event</th>
      <th>Journey</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>
      <details>
        <summary>web-store_game_open:</summary>
            <pre>
  "eventType": "web-store_game_open",<br/>
  "timestamp": 1675166796,<br/>
  "userId": "1234",<br/>
  "gameId": "gameX"<br/>
</pre>
        </details>
      </td>
      <td>Trigger `Web Store Game Open Events`</td>
      <td>Web Store sends the event whenever the user opens your game Web Store.</td>
    </tr>
    <tr>
      <td>
      <details>
        <summary>web-store_banner_show:</summary>
        <pre>
  "eventType": "web-store_banner_show",<br/>
  "timestamp": 1675166796,<br/>
  "userId": "1234",<br/>
  "gameId": "gameX",<br/>
  "bannerId": 123<br/>
</pre>
        </details>
      </td>
      <td>Action `Show Web Store Banner`</td>
      <td>Game should send the event to the Web Store (on <code>web-store_game_open/web-store_banner_click</code>) to define what banner should be displayed on Web Store.</td>
    </tr>
    <tr>
      <td>
      <details>
        <summary>web-store_banner_click:</summary>
        <pre>
  "eventType": "web-store_banner_click",<br/>
  "timestamp": 1675166796,<br/>
  "userId": "1234",<br/>
  "gameId": "gameX",<br/>
  "bannerId": 123
</pre>
        </details>
      </td>
      <td>Trigger `Web Store Banner Click Events`</td>
      <td>Web Store sends the event whenever the user clicks on the banner and redirects to the link set up for the banner (no event if banner is non-clickable).</td>
    </tr>
  </tbody>
</table>

## Integration via Journey

**Step 1**: “Integrate the Event Registry to start using Journey triggers out of the box

If you cannot use Event Registry, please configure Web Store trigger events via Journey triggers `.yaml` config for your game.

**Step 2**: Configure the Web Store action event `Show Web Store Banner` via Journey actions `.yaml` config for your game.

Add the following config instructions to your game `.yaml` config file (ID depends on your config):

```yaml
- id: XX
  name: Show Web Store Banner
  type: WEB_STORE
  fields:
    - name: BANNER_ID
      label: Banner Id
      type: PREDEFINED_STRING
      fieldKeyPath: bannerId
      predefinedValueSource:
        name: Banner Id
        urls:
          - "<web-store-service>/api/<some-path>"
      postProcessorScript: |-
        #redacted-script
      cacheable: false
      template: '{"bannerId": $BANNER_ID}'
      featureDevTeam: TBD
      responsiblePerson: TBD
```
