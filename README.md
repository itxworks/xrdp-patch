# xrdp-patch

Script based on:
http://www.c-nergy.be/blog

## Set Client IP from Session as Environmental Variable

https://github.com/neutrinolabs/xrdp/discussions/2221

```
--- a/sesman/session.c
+++ b/sesman/session.c
@@ -587,6 +587,16 @@ session_start_fork(tbus data, tui8 type, struct SCP_SESSION *s)
         }
         else if (window_manager_pid == 0)
         {
+            if (s->connection_description != NULL)
+            {
+                char ip[64];
+                g_get_ip_from_description(s->connection_description,
+                                          ip, sizeof(ip));
+                list_add_item(g_cfg->env_names,
+                             (tintptr) g_strdup("REMOTE_CLIENT_IP"));
+                list_add_item(g_cfg->env_values, (tintptr) g_strdup(ip));
+            }
+
             wait_for_xserver(display);
             env_set_user(s->username,
                          0,
```

### install

``` 

wget https://raw.githubusercontent.com/itxworks/xrdp-patch/main/xrdp-installer-1.3-itx.sh

chmod +x xrdp-installer-1.3-itx.sh

./xrdp-installer-1.3-itx.sh -c -s


```
