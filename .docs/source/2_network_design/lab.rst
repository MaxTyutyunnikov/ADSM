.. meta::
   :http-equiv=Content-Type: text/html; charset=utf-8

Лаба
====

Два вендора. Одна сеть. АДСМ.

Juniper + Arista. Ubuntu. Старая добрая Ева.

Количество ресурсов на нашей виртуалочке в Миране всё же ограничено, поэтому для практики мы будем использовать вот такую упрощённую до предела сеть. 

    .. figure:: https://fs.linkmeup.ru/images/adsm/2/lab.png
           :width: 800
           :align: center

* Два датацентра: Казань и Барселона.
* По два спайна в каждом: Juniper и Arista.
* По одному тору (Leaf'у) в каждом - Juniper и Arista, с одним подключенным хостом (возьмём легковесный Cisco IOL для этого).
* По одной ноде Edge-Leaf (пока только Juniper).
* One Cisco switch to rule them all.
* Помимо сетевых коробок запущена виртуальная машина-управляка. Под управлением Ubuntu.
    Она имеет доступ ко всем устройствам, на ней будут крутиться IPAM/DCIM-системы, букет питоновских скриптов, анзибль и что угодно ещё, что нам может понадобиться.

`Полная конфигурация <https://github.com/eucariot/ADSM/tree/master/docs/source/2_network_design/target_configs>`_ всех сетевых устройств, которую мы будем пробовать воспроизвести с помощью автоматики.
