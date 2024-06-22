<!-- Filename: J4.x:Namespace_Conventions_In_Joomla / Display title: Namensraum-Konventionen in Joomla -->

Joomla!  4.0

### Basis Namensräume

Die empfohlenen Namens-Konventionen für Joomla 4 sind folgende:

### Komponenten

#### Core-Komponenten

- Joomla\Component\\*ComponentName*\\*ClientName*
  - Joomla\Component\Content\Site
  - Joomla\Component\Content\Administrator

#### Komponenten von Drittanbietern

- *CompanyName*\Component\\*ComponentName*\\*ClientName*
  - Acme\Component\Backup\Site
  - Acme\Component\Backup\Administrator

#### Verzeichnis-Beispiele

*Joomla\Component\Content\Site\Controller*  
components/com_content/Controller/ArticleController.php

*Joomla\Component\Content\Administrator\Controller*  
administrator/components/com_content/Controller/ArticlesController.php

### Module

#### Core-Module

- Joomla\Module\\*ModuleName*\\*ClientName*
  - Joomla\Module\UsersLatest\Site
  - Joomla\Module\Quickicon\Administrator

#### Module von Drittanbietern

- *CompanyName*\Module\\*ModuleName*\\*ClientName*
  - Acme\Module\SubscriptionStatus\Site
  - Acme\Module\LastBackupStatus\Administrator

#### Verzeichnis-Beispiele

*Joomla\Module\Quickicon\Administrator\Dispatcher*  
administrator/modules/mod_quickicon/Dispatcher/Dispatcher.php

*Joomla\Module\UsersLatest\Site\Helper*  
modules/mod_users_latest/Helper/UsersLatestHelper.php

### Plugins

#### Core-Plugins

- Joomla\Plugin\\*PluginGroup*\\*PluginName*
  - Joomla\Plugin\System\Debug
  - Joomla\Plugin\Quickicon\Joomlaupdate

#### Plugins von Drittanbietern

- *CompanyName*\Plugin\\*PluginGroup*\\*PluginName*\>
  - Acme\Plugin\System\BackupOnDemand

#### Verzeichnis-Beispiele

*Joomla\Plugin\System\Debug*  
plugins/system/debug/DataFormatter.php

*Joomla\Plugin\Quickicon\Joomlaupdate\Extension*  
plugins/quickicon/joomlaupdate/Extension/Joomlaupdate.php

### Framework

#### Joomla Framework Bibliothek

- Joomla
  - Joomla\Session
  - Joomla\Image

#### Joomla CMS Bibliothek

- Joomla\CMS
  - Joomla\CMS\Mail
  - Joomla\CMS\MVC

#### Verzeichnis-Beispiele

*Joomla\Session*  
libraries/vendor/joomla/session/src/Session.php

*Joomla\CMS\Mail*  
libraries/src/Mail/Mail.php
