.. meta::
   :http-equiv=Content-Type: text/html; charset=utf-8

Введение и краткая история виртуализации
========================================

История современных технологий виртуализации берет свое начало в 1999 году, когда молодая компания VMWare выпустила продукт под названием VMWare Workstation. Это был продукт обеспечивающий виртуализацию desktop/client приложений. Виртуализация серверной части пришла несколько позднее в виде продукта ESX Server, который в дальнейшем эволюционировал в ESXi (i означает integrated) - это тот самый продукт, который используется повсеместно, как в IT так и в Telco, как гипервизор серверных приложений.

На стороне Opensource два основных проекта принесли виртуализацию в Linux:

* KVM (Kernel-based Virtual Machine) - модуль ядра Linux, который позволяет kernel работать как гипервизор (создает необходимую инфраструктуру для запуска и управления VM). Был добавлен в версии ядра 2.6.20 в 2007 году.
* QEMU (Quick EMUlator - отсюда и страус на лого) - непосредственно эмулирует железо для виртуальной машины (CPU, Disk, RAM, что угодно, включая USB порт) и используется совместно с KVM для достижения почти "native" производительности.

    На самом деле на сегодняшний момент вся функциональность KVM доступна в QEMU, но это не принципиально, так как бо́льшая часть пользователей виртуализации на Linux не использует напрямую KVM/QEMU, а обращается к ним как минимум через один уровень абстракции, но об этом позже.

Сегодня VMWare ESXi и Linux QEMU/KVM это два основных гипервизора, которые доминируют на рынке. Они же являются представителями двух разных типов гипервизоров:

* Type 1 - гипервизор запускается непосредственно на железе (bare-metal). Таковым является VMWare ESXi.
* Type 2 - гипервизор запускается внутри Host OS (операционной системы). Таковым является Linux KVM.

Обсуждение что лучше, а что хуже выходит за рамки данной статьи.

    .. figure:: https://fs.linkmeup.ru/images/adsm/1/1/hypervisors_types.gif
           :width: 800 px
           :align: center

Производители железа также должны были сделать свою часть работы, дабы обеспечить приемлемую производительность.

Пожалуй, наиболее важной и самой широко используемой является технология Intel VT (Virtualization Technology) - набор расширений, разработанных Intel для своих x86 процессоров, которые используются для эффективной работы гипервизора (а в некоторых случаях необходимы, так, например, KVM не заработает без включенного VT-x и без него гипервизор вынужден заниматься чисто софтверной эмуляцией, без аппаратного ускорения).
Наиболее известны два из этих расширений - VT-x и VT-d. Первое важно для улучшения производительности CPU при виртуализации, так как обеспечивает аппаратную поддержку некоторых ее функций (с VT-x 99.9% Gust OS кода выполняется  прямо на физическом процессоре, делая выходы для эмуляции только в самых необходимых случаях)
), второе для подключения физических устройств напрямую в виртуальную машину (для проброса виртуальных функций (VF) SRIOV, например, VT-d `должен быть включен <https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/virtualization_host_configuration_and_guest_installation_guide/sect-virtualization_host_configuration_and_guest_installation_guide-sr_iov-how_sr_iov_libvirt_works>`_).

| Следующей важной концепцией является отличие полной виртуализации (full virtualization) от пара-виртуализации (para-virtualization).
| Полная виртуализация - это хорошо, это позволяет запускать какую угодно операционную систему на каком угодно процессоре, однако, это крайне неэффективно и абсолютно не подходит для высоконагруженных систем.
| Пара-виртуализация, если коротко, это когда Guest OS понимает что она запущена в виртуальной среде и кооперируется с гипервизором для достижения большей эффективности. То есть появляется guest-hypervisor интерфейс.
| Подавляющее большинство используемых операционных систем сегодня имеют поддержку пара-виртуализации - в Linux kernel это появилось начиная с ядра версии 2.6.20.

| Для работы виртуальной машины нужны не только виртуальный процессор (vCPU) и виртуальная память (RAM), требуется также эмуляция PCI-устройств. То, есть по сути, требуется набор драйверов для управления виртуальными сетевыми интерфейсами, дисками и тд.
| В гипервизоре Linux KVM данная задача была решена посредством внедрения **virtio** - фреймворка для разработки и использования виртуализированных устройств ввода/вывода.
| Virtio представляет из себя дополнительный уровень абстракции, который позволяет эмулировать различные I/O устройства в пара-виртуализированном гипервизоре, предоставляя в сторону виртуальной машины единый и стандартизированный интерфейс. Это позволяет переиспользовать код virtio-драйвера для различных по своей сути устройств. Virtio состоит из:

* Front-end driver - то что находится в виртуальной машине
* Back-end driver - то что находится в гипервизоре
* Transport driver - то что связывает backend и frontend

| Эта модульность позволяет изменять технологии, применяемые в гипервизоре, не затрагивая драйверы в виртуальной машине (этот момент очень важен для технологий сетевой акселерации и Cloud-решений в целом, но об этом позже).
| То есть существует связь guest-hypervisor, когда Guest OS "знает" о том, что запущена в виртуальной среде.

    Если вы хоть раз писали вопрос в RFP или отвечали на вопрос в RFP "Поддерживается ли в вашем продукте virtio?" Это как раз было о поддержке front-end virtio драйвера.
