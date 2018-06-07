<h1 id="requires">requires</h1>


This is the requires side of the interface layer, for use in charms that wish
to request integration with OpenStack native features.  The integration will be
provided by the OpenStack integration charm, which allows the requiring charm
to not require cloud credentials itself and not have a lot of OpenStack
specific API code.

The flags that are set by the requires side of this interface are:

* **`endpoint.{endpoint_name}.joined`** This flag is set when the relation
  has been joined, and the charm should then use the methods documented below
  to request specific OpenStack features.  This flag is automatically removed
  if the relation is broken.  It should not be removed by the charm.

* **`endpoint.{endpoint_name}.ready`** This flag is set once the requested
  features have been enabled for the OpenStack instance on which the charm is
  running.  This flag is automatically removed if new integration features are
  requested.  It should not be removed by the charm.

<h1 id="requires.OpenStackIntegrationRequires">OpenStackIntegrationRequires</h1>

```python
OpenStackIntegrationRequires(self, endpoint_name, relation_ids=None)
```

Interface to request integration access.

Note that due to resource limits and permissions granularity, policies are
limited to being applied at the charm level.  That means that, if any
permissions are requested (i.e., any of the enable methods are called),
what is granted will be the sum of those ever requested by any instance of
the charm on this cloud.

Labels, on the other hand, will be instance specific.

Example usage:

```python
from charms.reactive import when, endpoint_from_flag

@when('endpoint.openstack.ready')
def openstack_integration_ready():
    openstack = endpoint_from_flag('endpoint.openstack.joined')
    credentials = openstack.get_credentials()
    update_config_enable_openstack(credentials)
```

<h2 id="requires.OpenStackIntegrationRequires.is_ready">is_ready</h2>


Whether or not the request for this instance has been completed.

