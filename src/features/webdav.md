# Webdav bridge

Peergos has a webdav bridge which you can run locally to allow any webdav-compatible client (including browsers) to talk to Peergos. Run the following command (replace peergos with "java -jar Peergos.jar" if you are using the jar):

> peergos webdav -peergos-url https://peergos.net -username $username -PEERGOS_PASSWORD $password -webdav.username $webdav-username -PEERGOS_WEBDAV_PASSWORD $webdav-password

Both passwords can be supplied via environment variables under the same name. 

You can then browse your peergos space at
http://localhost:8090/$YOUR-USERNAME

If you are using rclone you will need to enable a different auth scheme with:
> -webdav.authorization.scheme basic