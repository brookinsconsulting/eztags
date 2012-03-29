# eZ Tags extension installation instructions

## Requirements

   * eZ Publish 4.3+
   * eZ JSCore

## Optional extensions

   * eZ Find (used for tags suggestion and related tags feature)

## Upgrade

If upgrading the extension, see [doc/UPGRADE](/ezsystems/eztags/tree/multilanguage/doc/UPGRADE)

## Installation

IMPORTANT NOTE: There is a bug in eZ Image Editor 1.1 extension (ezie, released with eZ Publish 4.4)
that prevents eZ Tags datatype from functioning if your site is using jQuery version 1.4.3 and above.
This bug is fixed in extension version 1.2 (released with eZ Publish 4.5). To fix this, upgrade the
extension to 1.2, or downgrade your jQuery to 1.4.2
Bug fix commit is located [here](https://github.com/ezsystems/ezie/commit/6f29d071b8b100d62651ce8b696b97bf0f8f8b98)

### Unpack/unzip

Unpack the downloaded package into the `extension` directory of your eZ Publish installation.

### Create SQL tables in your eZ Publish database

Extension requires two additional tables to be added to your database. Use the following command from your eZ Publish
root folder, replacing `user`, `password`, `host` and `database` with correct values and removing double quotes

    mysql -u "user" -p"password" -h"host" "database" < extension/eztags/sql/mysql/schema.sql

### Activate extension

Activate the extension by using the admin interface ( Setup -> Extensions ) or by
prepending `eztags` to `ActiveExtensions[]` in `settings/override/site.ini.append.php`:

    [ExtensionSettings]
    ActiveExtensions[]=eztags

### Regenerate autoload array

Run the following from your eZ Publish root folder

    php bin/php/ezpgenerateautoloads.php --extension

Or go to Setup -> Extensions and click the "Regenerate autoload arrays" button

### Clear caches

Clear all caches (from admin 'Setup' tab or from command line).

### Speedup ajax calls

    1. Copy or symlink `index_treemenu_tags.php` from this extension to the root folder of eZ Publish (next to `index.php`)

    2. Add the following rewrite rule:

    .htaccess

        RewriteRule tags/treemenu/? index_treemenu_tags.php
        RewriteRule ^index_treemenu_tags\.php - [L]

    Virtual Host mode

        RewriteRule tags/treemenu/ /index_treemenu_tags.php [L]

### Allow anonymous users access to tags

For anonymous users to be able to see tags on your site, grant access to "view" function of "tags" module to Anonymous role

### (OPTIONAL) - If you wish to use tags suggestions and see related tags, you must use ezfind extension.

Edit the file `extension/ezfind/java/solr/conf/schema.xml` to add the following lines and then restart Tomcat/Jetty:

* Inside `<fields>` element add (without starting and ending quote):

    `<field name="ezf_df_tags" type="lckeyword" indexed="true" stored="true" multiValued="true" termVectors="true"/>`

    `<field name="ezf_df_tag_ids" type="sint" indexed="true" stored="true" multiValued="true" termVectors="true"/>`