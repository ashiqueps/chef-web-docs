+++
title = "Upgrade to Chef Automate 4.x"
date = 2022-03-03T12:02:46-08:00
draft = false
gh_repo = "automate"

[menu]
  [menu.automate]
    title = "Upgrade to 4.x"
    identifier = "automate/upgrade/major_upgrade.md Major Upgrade 4.x"
    parent = "automate/upgrade"
    weight = 30
+++

{{< warning >}} *Elasticsearch support has been removed from this version (4.0.27) of Automate.* {{< /warning >}}

Chef Automate provides an entire suite of enterprise capabilities for node visibility and compliance. Chef Automate upgrades from one minor version to another automatically. However, Chef Automate will not automatically upgrade to a major version. See the instructions below for manually upgrading Chef Automate from date-based versions to Chef Automate *4.x*.

## Upgrade Journey

Please choose the following upgrade journey based on your current version of Chef Automate. All of these upgrades are manual upgrades.

| Your Current Version | Upgrade To |
| -------------------- | ---------- |
| Any version before 20220329091442| 20220329091442|
| 20220329091442| 3.0.49|
| 3.0.49| 4.0.x

For example, if today you are on version *2021201164433*, your upgrade journey should be:

1. Manual upgrade to *20220329091442*
1. Manual upgrade to *3.0.49*
1. Manual upgrade to *4.0.x*

## Prerequisites

- **Plan your downtime:** This upgrade requires downtime. Before upgrading, set the environment to handle the downtime. Run `sudo chef-automate maintenance on` to turn on maintenance mode.
- **Backup Chef Automate database:** This Chef Automate version upgrades ElasticSearch. [Backup](/automate/backup/) your data before upgrading.
- **Current version should be 3.0.49:** If you are not on *3.0.49* version, do regular upgrades according to your topology.

## Upgrade to Version 3.0.49

Check your current version:

```sh
sudo chef-automate version
```

{{< note >}} If your server Version is less than *20220329091442*, refer to [Upgrade to 3.0.49]({{< relref "major_upgrade.md" >}}) page. {{< /note >}}

{{< note >}} If your server sersion is less than *3.0.49*, upgrade to latest minor version in 3 series. {{< /note >}}

### Normal and airgapped upgrade to 3.0.49

To upgrade to automate 3.0.49 follow the link: [upgrading to 3.0.49]({{< relref "major_upgrade.md" >}})

## Upgrade Path from 3.0.49 to 4.0.x

There are four possible scenarios to upgrade from 3.0.49 to 4.0.x version.

- [Chef Automate with Embedded Elasticsearch]({{< relref "#chef-automate-with-embedded-elasticsearch" >}})

- [Chef Automate with External Elasticsearch]({{< relref "#chef-automate-with-external-elasticsearch" >}})

- [Chef Automate in Air-Gapped Environment With Embedded Elasticsearch]({{< relref "#chef-automate-in-air-gapped-environment-with-embedded-elasticsearch" >}})

- [Chef Automate in Air-Gapped Environment With External Elasticsearch]({{< relref "#chef-automate-in-air-gapped-environment-with-external-elasticsearch" >}})

{{< note >}} Confirm whether your installation is using an external elasticsearch but running the `chef-automate config show` command. It `enable=true` is present in the `global.v1.external.elasticsearch` config setting, you are using a external elasticsearch. {{< /note >}}

{{< warning >}} You drive should have a minimum of sixty percent of free space to start the major version upgrade. {{< /warning >}}

{{< warning >}} Disable the **sharding** for automate running embedded elasticsearch.

Also, accept the checklist item asking permission to disable sharding. {{< /warning >}}

### Chef Automate With Embedded Elasticsearch

To upgrade Chef Automate with embedded Elasticsearch, follow the steps given below:

**Upgrade Chef Automate from version 3.0.49 to 4.0.x**

1. Start a major version upgrade:\

Here, you will be prompted to accept multiple Pre Upgrade checklist. Accept the actions before upgrade.

```sh
sudo chef-automate upgrade run --major
```

Once you are done with the upgrade, follow the steps post upgrade which are:

1. Check the upgrade status of Chef Automate:

```sh
sudo chef-automate upgrade status
```

2. Turn off maintenance mode:

```sh
sudo chef-automate maintenance off
```

3. Migrate your data from *ElasticSearch 6.8* to *OpenSearch 1.2.4*:

```sh
sudo chef-automate post-major-upgrade migrate --data=es
```

4. Verify whether all services are running:

```sh
sudo chef-automate status
```

5. Clear the old ElasticSearch 6.8 data if all the data is present in your upgraded Chef Automate.

```sh
sudo chef-automate post-major-upgrade clear-data --data=es
```

### Chef Automate with External ElasticSearch

To upgrade Chef Automate with external elasticsearch, follow the steps given below:

**Upgrade Chef Automate from version 3.0.49 to 4.0.x**

1. Start major version upgrade:

```sh
sudo chef-automate upgrade run --major
```

2. Check upgrade status is up-to-date

```sh
sudo chef-automate status
```

3. Upgrade your external *ElasticSearch 6.8* to *OpenSearch 1.2.4* manually. If you have configured *Host*, *Port*, *Username* or *Password* of ElasticSearch, patch the new configuration to use Chef Automate.

4. Turn off maintenance mode:

```sh
sudo chef-automate maintenance off
```

### Chef Automate in Air-Gapped Environment With Embedded ElasticSearch

**Upgrade Chef Automate from version 3.0.49 to 4.0.x**

To upgrade to 4.0.x, follow the steps below:

#### On Internet connected machine

1. Download latest CLI of Chef Automate.

```sh
curl https://packages.chef.io/files/current/latest/chef-automate-cli/chef-automate_linux_amd64.zip | gunzip - > chef-automate && chmod +x chef-automate
```

2. Create an Airgap Installation Bundle (AIB).

```sh
sudo ./chef-automate airgap bundle create
```

3. Copy the latest Chef Automate CLI (`chef-automate`) and AIB (`automate_4.0.x.aib`) to the air-gapped machine running Chef Automate.

#### On Air-Gapped machine running Chef Automate

1. Set your upgrade strategy to none in Chef Automate config. Confirm by the following code:

```sh
sudo ./chef-automate config show
```

2. Upgrade using new AIB and Chef Automate CLI:

```sh
sudo ./chef-automate upgrade run --airgap-bundle automate_4.0.x.aib --major
```

**Post Upgrade**

1. Check the upgrade status of Chef Automate, run the following command:

```sh
sudo chef-automate upgrade status
```

2. Turn off the maintenance mode.

```sh
sudo chef-automate maintenance off
```

3. Migrate your data from *ElasticSearch 6.8* to *OpenSearch 1.2.4*:

```sh
sudo chef-automate post-major-upgrade migrate --data=es
```

4. Verify whether all services are running:

```sh
sudo chef-automate status
```

5. Clear the old ElasticSearch 6.8 data if all the data is present in your upgraded Chef Automate.

```sh
sudo chef-automate post-major-upgrade clear-data --data=es
```

### Chef Automate in Air-Gapped Environment With External ElasticSearch

**Upgrade Chef Automate from version 3.0.49 to 4.0.x**

To upgrade to 4.0.x, follow the steps below:

#### On Internet connected machine

1. Download latest CLI of Chef Automate

```sh
curl https://packages.chef.io/files/current/latest/chef-automate-cli/chef-automate_linux_amd64.zip | gunzip - > chef-automate && chmod +x chef-automate
```

2. Create an Airgap Installation Bundle (AIB):

```sh
sudo ./chef-automate airgap bundle create
```

3. Copy the latest Chef Automate CLI (`chef-automate`) and AIB (`automate_4.0.x.aib`) to the air-gapped machine running Chef Automate.

#### On Air-Gapped machine running Chef Automate

1. Set your upgrade strategy to none in Chef Automate config. Confirm by the following code:

```sh
sudo ./chef-automate config show
```

2. Upgrade using new AIB and Chef Automate CLI:

```sh
sudo ./chef-automate upgrade run --airgap-bundle automate_4.0.x.aib --major
```

3. Check whether upgrade status is up-to-date

```sh
sudo chef-automate status
```

4. Upgrade your external *ElasticSearch 6.8* to *OpenSearch 1.2.4* manually. If you have configured *Host*, *Port*, *Username* or *Password* of ElasticSearch, patch the new configuration to use Chef Automate.

5. Turn off maintenance mode using following command:

```sh
sudo chef-automate maintenance off
```

{{< note >}}
After upgrading to version 4.x, Automate will continue to have both the configuration for OpenSearch and Elasticsearch. It is recommended to remove the Elasticsearch configuration after upgrading to External OpenSearch.
{{< /note >}}

## Troubleshooting

If Chef Automate fails to migrate your data to *OpenSearch 1.2.4* while running `chef-automate post-major-upgrade migrate --data=es`, restore the data using:

```sh
sudo chef-automate backup restore </path/to/backups/>BACKUP_ID
```

To restore your Air-Gapped bundle, run the following command:

```sh
sudo chef-automate backup restore  --airgap-bundle </path/to/bundle> </path/to/backups/>BACKUP_ID
```

Click [here](/automate/restore/) to know more about the Restore methods.

To start the upgrade, use the **backup ID** from the backup created. In case the restore fails even after upgrading the Chef Automate version, follow the steps given below:

1. Uninstall Chef Automate.

```sh
sudo chef-automate uninstall
```

2. Install the last major version (`3.0.49`) using the [air-gapped installation](/automate/airgapped_installation/) process.

3. Restore the backup:

```sh
sudo chef-automate backup restore <backup_id>
```

Refer to the [Chef Automate Restore](/automate/restore/) documentation.

{{< note >}} Remove the `/hab/svc/deployment-service/var/upgrade_metadata.json` file if the migration of data has been done using backup and restore method. {{< /note >}}
