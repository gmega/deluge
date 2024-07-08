# Webui Examples

**Tested and developed on deluge-svn/Deluge 1.1**
*updated with newer api for r.3857*


## Adding a page to the webui

### Intro
Webui templates are dumb, they get passed data from python, and can't call uiclient api's directly.

They are there to format the data, not to execute logic.

Template language: http://webpy.org/templetor

## Flow of the data
*Disclaimer, MVC is a highly abused therm, and i'm abusing it here.*

```
[core-plugin],deluged->uiclient->webui,[webui-plugin] -> data passed as arguments to template.
('Model'                       ) ('controller'       )   ('view')
```

## Example : a df page in webui
This example assumes you have deluge svn checked out to "~/src/deluge"
And that your plugin-development-directory is "~/prj/deluge/plugins"

This plugin will add a page to deluge that display the output of "df -h" to a page in the webui.


### Create a new plugin

```
cd ~/prj/deluge/plugins
python ~/src/deluge/deluge/scripts/create_plugin.py --name Df --basepath . --author-name "My Name" --author-email "deluge@example.com"
```

* Restart deluge(d) and enable the plugin in the gtk-ui.

### Add a core method.
Add this method to ~/prj/deluge/plugins/Df/df/core.py

```python
    def export_get_df(self):
        "returns the result of 'df -h' as a string"
        import commands
        return commands.getoutput('df -h')
```

### Test the new core-plugin method.
create ~/prj/deluge/plugins/df/Df/test.py ; Contents:

```python
from deluge.ui.client import sclient
sclient.set_core_uri()

print sclient.df_get_df() #export method is prefixed by plugin-name.
```

Terminal:

```
cd ~/prj/deluge/plugins/Df/df
killall deluged & deluged  && python test.py
```
Fix any errors until the test outputs the desired result.

### Add the template
~/prj/deluge/plugins/Df/df/templates/df.html

```
$def with (df_result)
<h1>df -h</h1>
<pre>
$df_result
</pre>
```
### Webui plugin
Edit ~/prj/deluge/plugins/Df/df/webui.py and replace the df_page and WebUI classes

Some Magic : api.render.<plugin-name>.<template-name>() is automagiclly mapped to templates/<template-name>.html (see http://webpy.org/ for templetor, the template language)

```python
class df_page():
    #@api.deco.deluge_page #requires  login, see page_decorators.py for more decorators,
    @api.deco.deluge_page_noauth #<-- this is easier for testing until i implement persistent sessions.
    def GET(self, args):
        df_result = sclient.df_get_df()
        return api.render.df.df(df_result) #push data to templates/df.html

class WebUI(WebUIPluginBase):
    #map url's to classes: [(url,class), ..]
    urls = [('/df', df_page)] #<--modified url mapping.

    def enable(self):
        api.config_page_manager.register('plugins', 'df' ,ConfigForm)

    def disable(self):
        api.config_page_manager.deregister('df')

```


* Remove template/default.html , it's not used in api.render.df anymore.

### Test webui page

```
killall deluge & deluge -u web &
```
Read the output to the console, there could be some errors there.

visit http://localhost:8112/df

Execute test until there are no more errors, and the page displays the desired result.

### Add to top menu
We added a page, but the user can't navigate to it, let's fix that :

Add 2 lines for register/deregister of the menu-items.

```python
    def enable(self):
        api.config_page_manager.register('plugins', 'df' ,ConfigForm)
        api.menu_manager.register_admin_page("df", _("Disk usage"), "/df") #<--top menu

    def disable(self):
        api.config_page_manager.deregister('df')
        api.menu_manager.deregister_admin_page("df") #<--top menu
```

### Pretty template
We're allmost ready, but the current template doesn't look right.

Adding a header/footer and menu to the template:

```
$def with (df_result)

$:render.header(_("Disk usage"), 'df')
$:render.admin_toolbar('df')
<div class="panel">
<h2>Disk usage (df -h)</h2>
<pre>
$df_result
</pre>'
</div>
$:render.footer()

```