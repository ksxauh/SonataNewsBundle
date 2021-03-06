API
===

SonataNewsBundle embeds a Controller to provide an API through ``FOSRestBundle``,
with its documentation provided by ``NelmioApiDocBundle``.

Setup
-----

If you wish to use it, you must first follow the installation instructions of both bundles:

* `FOSRestBundle <https://github.com/FriendsOfSymfony/FOSRestBundle>`_
* `NelmioApiDocBundle <https://github.com/nelmio/NelmioApiDocBundle>`_

Here's the configuration we used, you may adapt it to your needs:

.. configuration-block::

    .. code-block:: yaml

        # config/packages/fos_rest.yaml

        fos_rest:
            param_fetcher_listener: true
            body_listener:          true
            format_listener:        true
            view:
                view_response_listener: force
            body_converter:
                enabled: true
                validate: true

    .. code-block:: yaml

        # config/packages/sensio_framework_extra.yaml

        sensio_framework_extra:
            view:    { annotations: false }
            router:  { annotations: true }
            request: { converters: true }

    .. code-block:: yaml

        # config/packages/twig.yaml

        twig:
            exception_controller: 'FOS\RestBundle\Controller\ExceptionController::showAction'

In order to activate the API's, you'll also need to add this to your routing:

.. code-block:: yaml

    # config/routes.yaml

    NelmioApiDocBundle:
        resource: "@NelmioApiDocBundle/Resources/config/routing.yml"
        prefix:   /api/doc

    sonata_api_news:
        type:         rest
        prefix:       /api
        resource:     "@SonataNewsBundle/Resources/config/routing/api.xml"

Serialization
-------------

We're using JMSSerializationBundle's serializations groups to customize the inputs & outputs.

The taxonomy is as follows:

* ``sonata_api_read`` is the group used to display entities
* ``sonata_api_write`` is the group used for input entities (when used instead of forms)

If you wish to customize the outputted data, feel free to setup your own serialization options
by configuring JMSSerializer with those groups.
