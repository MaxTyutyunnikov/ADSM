.. meta::
   :http-equiv=Content-Type: text/html; charset=utf-8

Основы виртуализации
====================

    Автор этой статьи - `Роман Горге <https://www.linkedin.com/in/roman-gorge-2b15896b/?originalSubdomain=se>`_ - бывший ведущий linkmeup.

`Предыдущая статья <https://linkmeup.ru/blog/449.html>`_ рассматривала архитектуру виртуализированной сети, underlay-overlay, путь пакета между VM и прочее. В данной статье мы затронем (или попытаемся затронуть) вопросы а как собственно происходит виртуализация сетевых функций, как реализован backend основных продуктов, обеспечивающих запуск и управление VM, а также как работает виртуальный свитчинг (OVS и Linux bridge).

Тема виртуализации широка и глубока, объяснить все детали работы гипервизора невозможно (да и не нужно). Мы ограничимся минимальным набором знаний необходимым для понимания работы любого виртуализированного решения, не обязательно Telco.

    .. figure:: https://fs.linkmeup.ru/images/adsm/1/1/kdpv.png
           :width: 800
           :align: center

.. toctree::
   :maxdepth: 2
   :caption: Содержание:

   history.rst
   resource_types.rst
   vitual_switching.rst
   tools.rst
   conclusion.rst
