# Netbox IPAM
[[_TOC_]]

2 Server

http://10.244.2.88:8888 Production server. NETOPs team use this.

http://10.244.35.112:8888  Dev sever. Use for dev code and test before push to production server

Login account
user: root
password: default snt password

NetBox is an IP address management (IPAM) and data center infrastructure management (DCIM) tool. We use Netbox to keep the information of IP allocating, server, network inventory and customer's service information. etc

NetBox runs as a web application atop the Django Python framework with a PostgreSQL database.

We create the new forms for collecting customer's information base on services that we provide.
1.Cross-Connect (/opt/netbox/plugin_crossconnect)
 
2.BaaS (/opt/netbox/baas)

3.DRaaS (/opt/netbox/draas)

4.IaaS (/opt/netbox/iaas)

5.Blended IP (/opt/netbox/blendedip)

![plugins](images/netbox plugin.png)

### How to develop plugin

Please read this https://ttl255.com/developing-netbox-plugin-part-1-setup-and-initial-build/

This blog will explain you step by step how to develop the plugin but it's not completely the same as our service.

I'll show you the example how to customize our plugin for 1 example

### Plugin customization example

Example: We want to add new field name "Test add field" with character field box for IaaS plugin

1. Go to model.py in /opt/netbox/iaas/iaas/model.py and create parameter name "testaddfield" and import character field class to this parameter 
![create field.png](images/create field.png)

Then Save

2. Run terminal run this command "source /opt/netbox/venv/bin/activate" for enable virtual environment mode

![image.png](images/virtual env.png)

It should have (venv) on the front of your directory path

3. Go to /opt/netbox/netbox/iaas/ and run "python3 setup.py install" to apply the field "testaddfield"

![image.png](images/install.png)

The result should have no error like above.

4. Go to /opt/netbox/netbox/ and run "python3 manage.py makemigrations iaas" to detect change/new field creating on /iaas/model.py and generate to template to apply for database


It should generate the template file like this

![image.png](images/generatemakemigration.png)

Inside template file look like this

![image.png](images/generatemakemigration2.png)

5. Run "python3 manange.py makemigrate iaas" to apply the template that we just created to database

![image.png](images/migrate.png)

6. Copy the template file that we created to /opt/netbox/iaas/iaas/migrations to keep the continuing sequence and keep tracking new model.py change

![image.png](images/copyfile.png)

7. Go to /opt/netbox/iaas/iaas/form.py and add field "testaddfield" in class "iaasForm"

![image.png](images/form.png)

Then Save and run "/opt/netbox/netbox/iaas/python3 setup.py install" to apply the changing on form.py

8. Let's take a look on Plugins -> IaaS and click add or edit. We'll see the new field appear on form

![image.png](images/form2.png)

9. In case we want to show that field on the list page. Go to /opt/netbox/iaas/iaas/tables.py and add "testaddfield" to it

![image.png](images/tables.png)

Then save. And Don't forget to run python3 setup.py install to apply the changing. We'll see the new column on list page.

![image.png](images/listpage.png)

