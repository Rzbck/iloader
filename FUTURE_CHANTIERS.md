# Future chantiers

## iLoader over Wi-Fi after initial USB pairing

Goal: allow iLoader to use a previously paired/trusted iPhone over the local network instead of requiring USB for every install.

Scope:
- initial USB pairing/trust is acceptable;
- subsequent discovery/connection/install should work over Wi-Fi when the device is reachable;
- preserve exact device selection and never silently switch devices/transports;
- keep USB as a reliable fallback;
- do not target Bluetooth as the primary installation transport;
- audit and reuse the existing `idevice` provider/transport capabilities first (TCP/network discovery, pairing prerequisites on Windows);
- keep this chantier separate from Apple Watch installation/coordinator fixes.

Status: note only; no implementation started.
