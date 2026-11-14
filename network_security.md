# Отчет по лабораторной работе 15. Безопасная передача данных

## 1. Network Security Config (NSC)
```xml
<network-security-config>
    <base-config cleartextTrafficPermitted="false">
        <trust-anchors>
            <certificates src="system" />
        </trust-anchors>
    </base-config>
    <domain-config cleartextTrafficPermitted="false">
        <domain includeSubdomains="true">api.tracker.example</domain>
        <pin-set expiration="2026-06-01">
            <pin digest="SHA-256">base64(СПИСОК_СПКИ_ПРОМЕЖУТОЧНОГО)</pin>
            <pin digest="SHA-256">base64(СПИСОК_СПКИ_КОРНЕВОГО)</pin>
        </pin-set>
    </domain-config>
    <debug-overrides>
        <trust-anchors>
            <certificates src="system" />
            <certificates src="user" />
        </trust-anchors>
    </debug-overrides>
</network-security-config>
