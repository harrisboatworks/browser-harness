# Google Ads change-history audits

## Routes and table loading

- Direct route: `https://ads.google.com/aw/changehistory`.
- Change History uses a virtualized table inside the app scroll host. If the footer reports rows but `document.body.innerText` shows only headers, scroll the `div.awsm-nav-bar-and-content` element. The event rows then render as top-level `.particle-table-row` elements.
- One recommendation type can produce several events. Group the visible rows by the `Applied recommendation:` label, but preserve every event timestamp when reporting what happened.

## Expanding exact changes

- Each top-level event can contain a second, visible `material-icon[aria-expanded="false"]` for the affected resource. The first icon is often hidden and must not be used.
- The reliable expansion sequence is: focus the visible `material-icon`, then send Enter with `press_key("Enter")`. The expanded child row exposes the exact keyword, target, budget, or other changed value.
- Confirm expansion by reading `aria-expanded="true"` and by checking that the new child text appeared. A coordinate click on the glyph may be ignored.

## Date-picker safety

- Recommendation cards and report date pickers both contain buttons labelled `Apply`. Never locate an Apply button by text alone on Google Ads pages.
- Open the date picker from its displayed range, then edit only `date-range-input.range input` elements.
- After dispatching `input` and `change` events, target the visible `material-button.apply` inside the date-picker overlay (`offsetParent !== null`). Focus it and press Enter.
- If the date-picker Apply button is clipped, temporarily increase the CDP viewport height with `Emulation.setDeviceMetricsOverride`; do not fall back to the first visible Apply button.
- Verify the displayed date range after applying it before reading metrics.

## Read-only verification

- After any ambiguous interaction, revisit Change History and verify that no event was created at the audit time before continuing.
- Auto-apply settings are available at `https://ads.google.com/aw/recommendations/autoapply`. The History tab shows each recommendation type, current On/Off state, total applications, and last-applied date.
