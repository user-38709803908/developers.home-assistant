---
title: "Repair issues and repair flows are used when user intervention is needed"
---

## Reasoning

Repair issues and repair flows are a very user-friendly manner to let the user know something is wrong and that they can do something about it.
Repair issues are just a way to let the user know that they can fix it themselves, while repair flows can fix it for them.

Repair issues and repair flows should be actionable and informative about the problem.
Thus, we should not raise repair issues for just letting users know that something is wrong, which they can't fix themselves.

## Example implementation

In the example below we have an integration for a locally hosted service.
On boot, we check if we support the version of the service that is running.
If we do not, we raise a repair issue where we let the user know that they should update their service before they can use the integration again.

`__init__.py`
```python {6-14} showLineNumbers
async def async_setup_entry(hass: HomeAssistant, entry: MyConfigEntry) -> None:
    """Set up the integration from a config entry."""
    client = MyClient(entry.data[CONF_HOST])
    version = await client.get_version()
    if version < MINIMUM_VERSION:
        ir.async_create_issue(
            hass,
            DOMAIN,
            "outdated_version",
            is_fixable=False,
            issue_domain=DOMAIN,
            severity=ir.IssueSeverity.ERROR,
            translation_key="outdated_version",
        )
        raise ConfigEntryError(
            "Version of MyService is %s, which is lower than minimum version %s",
            version,
            MINIMUM_VERSION,
        )
```

## Additional resources

For more information about repair issues and repair flows, see the [repairs](/docs/core/platform/repairs) documentation.

## Exceptions

There are no exceptions to this rule.