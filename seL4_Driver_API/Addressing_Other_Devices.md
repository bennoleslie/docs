= seL4 Driver API Persistent Device Naming Scheme =

Recommended prior reading for this page is the seL4 Driver API's [[seL4 Driver API/Child Enumeration#Addressing Names|location-based device addressing scheme]].

== Contents ==
<<TableOfContents()>>

== Constants ==

== Functions ==
{{{
seL4drv_connection_t *seL4drv_external_connect();
}}}
== Data structures ==
{{{
typedef void seL4drv_connection_t;
}}}

== Description ==

The seL4 Driver API requires the use of a persistent naming scheme which forms the basis of the IPC connection system between device instances. It is through this naming scheme that a device instance can call to its parent devices, and communicate with devices with which it has lateral dependencies.

Devices' addressable names are generated by the parent driver that enumerates them. Child device enumeration is discussed in detail in the [[seL4 Driver API/Child Enumeration#Addressing Names|Child Enumeration]] article.

A driver connects to an external device that it depends on by calling `seL4drv_external_connect()`, passing in the addressable name of the particular device that it wishes to connect to. The environment shall search its device tree for a device which has that name, and return a handle of type `seL4drv_connection_t *`. Notice that `seL4drv_connection_t` is of type `void` (see [[#Data Structures|above]]: this is because it is an opaque handle, and the driver is not meant to dereference this handle after receiving it. This handle is meant to be passed to subsequent function calls to the particular API that was connected to in order to maintain context between the driver and the target device.

If the environment is unable to locate a device whose addressable name matches the one supplied in the call to `seL4drv_external_connect()`, it shall return '''NULL'''. The calling driver shall take this to mean that the device is trying to connect to does not exist, or that it exists but the environment does not have a driver that can drive it, and so is unusable.