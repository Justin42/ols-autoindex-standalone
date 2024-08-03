A modified version of the default OpenLiteSpeed index script that can be used in a normal user context.
This removes any dependency on OLS auto indexing. Additional features to better support this use may be added in the future.

***Please note that this has not been audited for security in any way.***

### Use cases -
- Full user customization of auto indexing.
- Per-directory indexing without the use of `Options` overrides. Use with Vhost default `_autoindex` context or simple rewrites in `.htaccess`.
- Use OpenLiteSpeed auto-indexing script on other web servers.

### Known issues -
- Some data such as MIME types and file exclusions that would normally be provided by OLS have not yet been re-implemented and are not available in this way.


### User Installation -
- Upload `dist/share/autoindex` to domain document root at `/share/autoindex`
- Add rewrite to .htaccess in directories to enable indexing.
```
<IfModule rewrite_module>
    RewriteEngine on
    RewriteCond %{REQUEST_FILENAME} -d
    RewriteRule .* /share/autoindex/default.php
</IfModule>
```
- Customize as desired via `UserSettings` in `autoindex_include.php`

*This will require your host to have mod_rewrite enabled and set to auto-load from .htaccess.*

### Managed installation -
- Upload to shared server path such as `/lsws/share/autoindex_custom`
- Modify Vhost default `_autoindex` context to point to the chosen path. `$SERVER_ROOT/share/autoindex_custom`.  
- Customize as desired via `UserSettings` in `autoindex_include.php`

#### To force enable for all directories:
- Enable auto indexing at the vhost level
- Ensure AutoIndexURI points to the location. `/lsws/share/autoindex_custom/default.php`

#### To enable for user selected directories:
- Place .htaccess with rewrite to `/_autoindex/default.php` in desired directories (applies to subdirectories).
```
<IfModule rewrite_module>
    RewriteEngine on
    RewriteCond %{REQUEST_FILENAME} -d
    RewriteRule .* /_autoindex/default.php
</IfModule>
```

Alternatively, a rewrite for a static context can be used instead of .htaccess. See [OpenLiteSpeed Docs *From Document Root .htaccess to VirtualHost Context tab*](https://docs.openlitespeed.org/config/rewriterules/#from-document-root-htaccess-to-virtualhost-context-tab) and [OpenLiteSpeed Docs *Auto-Index - Context Level*](https://docs.openlitespeed.org/config/advanced/autoindex/#context-level). 
This is generally less preferable as it allows less control for the user. This, combined with the lack of `Options` override support in OLS should be the primary motivation for using any custom auto-indexing script.

A similar goal may be able to be achieved with the unmodified OLS indexing script via a combination AutoIndexURI and .htaccess rewrites, without enabling indexing for the entire Vhost. This has not yet been tested and is otherwise undocumented by OLS. 