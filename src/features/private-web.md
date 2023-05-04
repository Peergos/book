# Private Websites

You can turn any folder in Peergos into a private website, benefitting from the built-in access control and privacy. Such websites can be viewed using a built-in browser [app](./apps.md). This browser isolates websites from different owners using different sub domains just as different apps are themselves isolated. Such websites are locked down such that external communication is impossible [0]. This means that 3rd party tracking is impossible. The beautiful thing here is that 1st party tracking is also not easy (and can be made impossible) because the paths are resolved locally in the browser and all requests go through the peergos server you are connecting to. This means browser fingerprinting is irrelevant for such websites because no information can be exfiltrated!

You can share websites privately with friends on peergos the same way you share any file or folder. You can even share them with anyone via a secret link!

A private website can also link to any other websites in Peergos, including those owned by others, by the human readable path in the Peergos global filesystem. Following such a link will only work if you also have read access to the destination. 

The possibilities are huge here for a better, more private web that protects people from surveillance. You can also edit your website's directly in Peergos. It has never been easier to host your own website securely!

[0] This is currently not true until browsers implement the [webrtc CSP](https://github.com/w3c/webappsec-csp/issues/92) which allows us to block any webrtc connections which can be used to exfiltrate data. 