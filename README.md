# odoo-docker-tutorial-15

odoo15, python 版本是 3.8+, postgres 版本是 13

其他教學請參考 [master分支](https://github.com/twtrubiks/odoo-docker-tutorial)

說明 [odoo.conf](https://github.com/twtrubiks/odoo-docker-tutorial/blob/15.0/config/odoo.conf)

基本上都可以在 source code 中的 `odoo/tools/config.py` 找到,

`--stop-after-init`

代表 `stop the server after its initialization.`

通常這個參數會搭配測試或是升級使用, 檢查是否有錯誤.

`--without-demo`

代表 不要載入 demo data.

`--unaccent`

代表 `Try to enable the unaccent extension when creating new databases.`

這是 Postgresql 中的 unaccent extension, 預設是 False.

`--transient-age-limit`

代表 transientModel 自動刪除的時間, 預設是 1 小時.

```text
Time limit (decimal value in hours) records created with a TransientModel (mostly wizard) are kept in the database. Default to 1 hour.
```

`--osv-memory-count-limit`

代表 transientModel 最大的 record 數量.

```text
Force a limit on the maximum number of records kept in the virtual osv_memory tables. By default there is no limit.
```