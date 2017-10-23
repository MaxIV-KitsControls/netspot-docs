# Templates

Templates are small configlets or complate configs that can be used as input to any playbook. The templates use a rich template language used by Django. For more information about the language please see the [Django templates](https://docs.djangoproject.com/en/1.11/ref/templates/).

## Example template

```
interfaces {
    {{ interface }} {
        description "{{ description }}";
        unit 0 {
            family ethernet-switching;
        }
    }
}
```

This template uses two variables or inputs - *interface* and *description*.

The variables/inputs can be added in the template configuration. 

## Add new template

Click on "Add new template" on the template page in the NetSPOT webgui.

The field *Playbook* is used to specify a default playbook to be used with the template.

### Template variables

The variables in the configuration can be used in the template it self. It's possible to specify a regex that's used to for input validation. The example field is to give the user an example or explanation of what the variable is for.

## Deploy ports templates

There are special templates. All templates under the **Network ports** will be avaible in the dropdown on asset interfaces. These **Network ports** templates need at least one variable configured - *interface*. This field will automatically be filled in by the system when deploying ports.