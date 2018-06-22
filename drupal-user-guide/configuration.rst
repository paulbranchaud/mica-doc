Drupal Configuration
====================

Drupal is turned into Mica Drupal Client via a set of Drupal modules that can be enabled/disabled in the Modules > OBiBa subsection of Drupal.

.. note::
  If you decide to disable one of OBiBa Drupal module, make sure you know exactly what it does. As a general rule, all modules should be enabled in order to make Mica Client works. There are, however, two notable exceptions to this rule:

  You may disable both the "Data Access Request" and the "OBiBa Auth" modules in the case you don't intend to use the Data Access Request feature provided by Mica.
  Not an OBiBa module per se, but one which Mica Client use extensively is the Google Chart module (in the Chart section). If you intend to use Highcharts in your portal, you may want to activate the module there and disable the Google Chart modules.

OBiBa Mica settings
-------------------

Here, we will explain how to configure Mica's services. The sections enumerated here reflect the sections present in the section Configuration > OBiBa Mica settings of the administration panel.

OBiBa Study Server (MICA)
~~~~~~~~~~~~~~~~~~~~~~~~~

This subsection lists various fields that Mica Drupal Client uses to communicate with Mica Server. Here is a succinct description of each fields along with its name:

================================================ ===============================================
Field                                            Description
================================================ ===============================================
Mica address                                     The URL of Mica Server
Anonymous user name                              The Anonymous user has read permission upon the content that has been published on Mica server. Here, you enter the name of the anonymous user as know by Mica Server.
Anonymous user password                          Self-explanatory.
Copyright Notice Text                            A copyright notice to be included if a user download a list of data.
Number of items per server response page	       Determines the how many items that must be displayed in a server response page. For instance, this parameter affects the number of variables listed in a page.
Minimum number of items per server response page Determines the minimum number of items that must to be displayed in a server response page. This parameter affects the number of studies, networks or datasets listed on a page.
================================================ ===============================================

Data Access Request
~~~~~~~~~~~~~~~~~~~
Either the name of a field is self-explanatory or the explanation located below that field is sufficient to understand what it is meant for except for the last item:

Access request commenting. If checked, data access request commenting is enabled. For a given Access Request form, there will be a comment tab aside the history tab. By checking on this option, the commenting area can be used for a discussion between the Data Access Officer (DAO) and the user who request access.

Statistics Settings
~~~~~~~~~~~~~~~~~~~

The explanation that lies below the checkboxes is self-explanatory.

Cache Image settings
~~~~~~~~~~~~~~~~~~~~

The option for time image timeout is supposed to be clear. Now, you also have a button to clear the image cache. This might be useful as, for instance, logo of studies (or networks) don't tend to change much, so the image cache timeout tends to be long. If, however, you change an image, you can clear the cache right away.

Networks, Studies, Datasets and Variable Search
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Depending on the purpose for which you intend to use Mica, you might want to deactivate the Networks (resp. Studies, Datasets or Variables) tab in the Search page. By deactivating the checkbox aside Show Networks (resp. Studies, Datasets or Variables) search, the Network (resp. Studies, Datasets or Variables) tab won't show up in the Search page.

Below each of these four configurations (Show Networks, Studies, Datasets or Variables search) are options to customize the result of a given search string entered in the left-hand-side column e.g., to show or not the studies in the results when one search for a network.

In **Datasets Search > Show dataset auto-complete search filter**. If selected, the auto-complete search filter will be displayed in the search page. By choosing "Checkbox" you have a checkbox selection. Finally, you can also dispable the display.

Study, Dataset and Variable Content
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

By clicking on a "specific" result on the Search page, that is, not a number of networks, variable, studies or dataset, you are brought on a page that describes that network, study, dataset or a variable. In the configuration panel, the options listed in the Study, Dataset or Variable Content boxes will set options concerning the display of information on a description page of that type.

Taxonomies
~~~~~~~~~~

In this block, you may edit the appearance as well as the order of the taxonomies appearing in:

* **Figures**. This concerns the display (or its absence thereof) of all figures concerning Variable Classification e.g., the Area of information, the various constructs etc.
* **Search**. This concerns the display of the search panel (on the left) on the Search page under the Variable tab.

If the text area for **Taxonomy in Figure** is empty, it will display all taxonomies. This is the default state.

Translation
~~~~~~~~~~~

The last section is for translation of the web data portal created via Mica Drupal Client. The textarea concerns the pages that should not be translated. Suppose that your data web portal is translated in 2 languages (the primary language is English) and that a data access form for the data displayed therein is available only in English. Then, you can translate all the portal into the second language but not the pages related to data access. In order to do so, you need to enter the path of each of the page you don't want to be translated into the textarea separated by a coma and you're done: these pages will remain only in English.

Mica Drupal Client Templating
-----------------------------

We will examine two distinct ways to do templating: with a sub-theme and with a custom module.

Dependencies
~~~~~~~~~~~~

First of all, you need to get:

* `Bootstrap theme <https://www.drupal.org/project/bootstrap>`_
* `OBiBa Bootstrap sub-theme <https://www.drupal.org/project/obiba_bootstrap>`_

Further, see the `Drupal Bootstrap Documentation <https://drupal-bootstrap.org/api/bootstrap/7>`_.

Overriding templates via a new sub-theme
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Overriding a template is useful if one wants to determine the way the information is displayed in a page and have a better control over the design. Thus, for every page to display in Mica Drupal Client, there is a file (or a set of) template file(s) located in the corresponding template repository of each OBiBa module.

It is not recommended to modify these files directly or the modifications will be overwritten the next time OBiBa Modules will be updated thus the idea of template overriding.

.. note::

  The list of templates that we can override can be seen in the template.php file of obiba_bootstrap.

You may do template overriding as follow:

* First, create a sub-template as decribed in the documentations hyperlinked above
* Define obiba_bootstrap as the base theme in the .info file of that sub-theme.

Once the sub-theme is set, you can override the different vues generated by a module by copying the template file for that module in the template folder of that sub-theme, that is:

.. code-block:: bash

  cp /site/all/modules/obiba_mica/<module to overide>/templates/<template to overide> <drupal>/sites/all/themes/<Sub_theme_bootstrap>/templates

Overriding templates via custom module
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you want to use default template obiba_bootstrap, which entails making smaller edits to the design, you may override the templates in a custom module that you can install in your instance of Mica Drupal Client:

* Copy the template that you want to override in the folder "Template" of the custom module,
* Use the hook_theme() function to override the templates.

For instance, you can use the following in a .module file:

.. code-block:: php


  /*
  * hook_theme()
  */
  function MYMODULE_theme($existing, $type, $theme, $path){
   $theme = array();
        $theme['obiba_mica_dataset-detail'] = array(
          'template' => 'obiba_mica_dataset-detail',
          'path' => drupal_get_path('module', 'MYMODULE') . '/templates',
         );
        return $theme;
  }
