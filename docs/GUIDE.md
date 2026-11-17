# NetRouter guide

[فارسی](GUIDE.fa.md) · [Back to README](../README.md)

Everything you need to use NetRouter and fix problems is here. If a program does not use the connection you chose, start with section 4.

## 1. Quick start

1. If the orange "ForceBindIP is not installed" bar shows at the top, press **Install automatically**.
2. In the **Running** tab find your program. If it is not open yet, drag its .exe into "My programs".
3. Pick the connection in the dropdown beside it and press **Move** (or ▶ Run).
4. After a few seconds NetRouter verifies the result and tells you either "verified" or that the program is still on the old connection.

## 2. Best way to use it (for gamers)

Keep your main, fast connection (e.g. fiber) as the Windows default. Games then use it untouched. Move only background programs such as Steam, game launchers and download managers to the second connection (e.g. ADSL or Wi-Fi) so your ping does not spike mid-game.


If you move Steam, do it before you start a game. A game started by Steam stays on the default connection and is not affected.

## 3. "Move" or "Route by destination"?

**Move:** closes the program and restarts it on the chosen connection using ForceBindIP. No administrator needed, but it does not work on programs that do their networking in a helper process.


**🧭 Route by destination:** the program is not restarted. NetRouter finds the server addresses the program is connected to and routes only those through the chosen connection. It works on launchers and multi-process programs. Needs administrator.

## 4. The app says it switched, but the program is still on the old connection

1. Open NetRouter as administrator and in the **Diagnose** tab press "Run diagnosis".
2. If **both connections show the same public IP**: either the two modems share a subnet (section 6), both really are the same line, or Weak Host is on (section 11).
3. If the **public IPs differ**: Windows is fine and the problem is the program itself. Follow section 5 (route by destination).
4. If the diagnosis reports a **proxy or VPN**: read section 8.

## 5. Route by destination (full guide)

1. Open NetRouter with "Run as administrator".
2. Start the program and begin its download or work so it has connections.
3. In the Running tab, the rows with many connections are at the top. Choose the connection and press **🧭 Route by destination**. If several related programs have connections (e.g. a launcher and its downloader), do it for each.
4. Press **Pause then Resume** on the download once so it opens fresh connections through the new network.
5. The speeds at the top of the window should swap.


**Why does it sometimes slip back to the old connection for a moment?** Programs sometimes connect to new server addresses, and NetRouter has to see an address before it can route it. The first connection may start on the old network or reconnect once. NetRouter remembers the addresses it has seen and routes them from the start next time, so this happens much less after a few minutes or on the second run.


**Limits:** IPv4 and TCP only (not UDP or QUIC). Routes are removed when NetRouter closes. To remove all rules: Settings, "Clear all". The route applies to every program that connects to the same address.

## 6. Two modems on the same subnet (very common)

Many modems use the address 192.168.1.1. If your ADSL modem and your fiber modem both do, Windows gets confused between the two adapters and no tool works properly. The Diagnose tab shows a "same subnet" warning.


**Fix:** open the admin page of one modem (usually 192.168.1.1 in a browser), change the modem's LAN address to 192.168.2.1 in the LAN or DHCP section, save, restart the modem and reconnect.

## 7. Error 740 and administrator rights

This error means the target program itself requires administrator rights. Reopen NetRouter with "🛡 Run as administrator" (sidebar). Administrator mode is also needed for route-by-destination and for closing programs that run elevated.


In administrator mode, drag and drop from Explorer does not work (a Windows restriction); click the add box and choose the file instead. Start with Windows is not elevated automatically either.

## 8. Proxy and VPN (V2Ray, Clash and similar)

When a proxy or VPN is on, program traffic enters it first and the VPN app then goes out over the default connection. Choosing a network for the program then has no effect, because the connection is made by the VPN app, not by the program.


To test, turn off the VPN and the system proxy. If you need it, choose the outbound connection inside the VPN app itself. Tailscale is usually harmless.

## 9. Games with anti-cheat

Do not launch games such as Valorant, or any game using Easy Anti-Cheat, BattlEye or similar, with "Move" or "Run". DLL injection may be mistaken for cheating and get you banned. Leave the game untouched on the default connection and move only the launcher and downloader. Route by destination does no injection, but it is still best not to use it on the game itself. You use this at your own risk.

## 10. Browsers

Chromium browsers (Chrome, Edge, Brave and similar) run networking in a separate process, so "Move" does not work on them. "Route by destination" may work, but most people do not need it.

## 11. Weak Host

If the diagnosis says Weak Host is enabled, run this in an administrator CMD for both adapters (replace the name with the name you see in NetRouter):


```
netsh interface ipv4 set interface "Wi-Fi" weakhostsend=disabled weakhostreceive=disabled
```

## 12. SmartScreen and antivirus warnings

The installer is not code-signed, so Windows shows a warning. Click **More info** then **Run anyway**. Some antivirus programs may raise a false alarm because the app runs PowerShell and an injection tool.

## 13. The HUD is not visible over my game

The HUD cannot appear over true Exclusive Fullscreen games. Set the game to **Borderless Windowed** in its graphics settings.

## 14. FAQ

**Which connection does DNS use?** Always the Windows default connection.


**Do I have to launch the program from NetRouter every time?** For Move/Run, yes. Save the program in My programs and press ▶. Route by destination works by itself every time the program opens, as long as NetRouter is running as administrator.


**After a Windows restart?** Destination routes are cleared and are rebuilt automatically once NetRouter is running and the program opens. You can enable "Start with Windows" in Settings.
