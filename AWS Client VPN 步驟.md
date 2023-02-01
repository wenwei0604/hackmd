# AWS Client VPN 步驟

產生伺服器和用戶端憑證及金鑰並上傳到 ACM
開啟 EasyRSA 版本頁面，下載並擷取適用於您 Windows 版本的 ZIP 檔案。

開啟命令提示，然後導覽至 EasyRSA-3.x 資料夾被擷取到的位置。

執行下列命令以開啟 EasyRAS 3 殼層。

```
C:\Program Files\EasyRSA-3.x> .\EasyRSA-Start.bat
```
初始化新的 PKI 環境。
```
# ./easyrsa init-pki
```
若要建置新的憑證授權機構 (CA)，請執行此命令並依照提示執行。
```
# ./easyrsa build-ca nopass
```
產生伺服器憑證和金鑰。
```
# ./easyrsa build-server-full server nopass
```
產生用戶端憑證和金鑰。
```
# ./easyrsa build-client-full client1.domain.tld nopass
```
將伺服器憑證和金鑰及用戶端憑證和金鑰複製到自訂資料夾，然後導覽到自訂資料夾。

複製憑證和金鑰之前，請使用 mkdir 命令建立自訂資料夾。下列範例會在您的 C:\ 磁碟機中建立自訂資料夾。
```
mkdir C:\custom_folder
copy pki\ca.crt C:\custom_folder
copy pki\issued\server.crt C:\custom_folder
copy pki\private\server.key C:\custom_folder
copy pki\issued\client1.domain.tld.crt C:\custom_folder
copy pki\private\client1.domain.tld.key C:\custom_folder
cd C:\custom_folder
```

ovpn組態檔範例
```
client
dev tun
proto udp
remote cvpn-endpoint-00d0b595d3058e280.prod.clientvpn.ap-southeast-1.amazonaws.com 443
remote-random-hostname
resolv-retry infinite
nobind
remote-cert-tls server
cipher AES-256-GCM
--cert "C:\\custom_folder\\client1.domain.tld.crt"
--key "C:\\custom_folder\\client1.domain.tld.key"
verb 3
<ca>
-----BEGIN CERTIFICATE-----

-----END CERTIFICATE-----

</ca>


reneg-sec 0
```