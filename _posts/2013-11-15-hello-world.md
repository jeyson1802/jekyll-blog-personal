---
layout: post
title: Hello Posita!
categories: [general, setup, demo]
tags: [demo, dbyll, dbtek, setup]
fullview: true
comments: true
---

<div class="imageblock" style="text-align: center">
<div class="content">
<img src="../../../images/gradle.png" alt="Gradle" width="100">
</div>
</div>
<div class="paragraph">
<p>Viernes 6 pm, te alistas para dejar la oficina y disfrutar del fin de semana, esperas la finalización de la ejecución de los tests en el servidor de <code>Integración Continua</code> para irte con tranquilidad. Por tu cuenta, ya has ejecutado los tests individualmente y todo anda ok, cuando derrepente recibes una notificación con el asunto <code>BUILD FALLIDO…&#8203;</code>. Por alguna razón, el build ha fallado y tienes que verificar el por qué? Afortunadamente, las build tools ofrecen un método para poder hacer debugging remoto.</p>
</div>
<div class="paragraph">
<p>En esta oportunidad, daremos a conocer como <code>Gradle</code> permite debugear cualquier tarea haciendo uso de la siguiente sintaxis <code>&lt;taskName&gt;.test</code>.</p>
</div>
<div class="paragraph">
<p>En el ejemplo, haremos uso del proyecto <a href="https://github.com/eddumelendez/ldap-spring-boot" target="_blank" rel="noopener">ldap-spring-boot</a>. Primero, colocaremos un punto de interrupción en la clase <code>Application.java</code> del subproyecto <code>ldap-spring-boot-samples/odm</code>.</p>
</div>
<div class="paragraph">
<p>Luego, ejecutaremos el siguiente comando:</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="pygments highlight"><code data-lang="bash">./gradlew -Dtest.debug :ldap-spring-boot-samples/odm:test</code></pre>
</div>
</div>
<div class="paragraph">
<p><strong>NOTA:</strong> Para propósitos del ejemplo sólo ejecutaremos la tarea test del subproyecto en mención.</p>
</div>
<div class="paragraph">
<p>Después de la ejecución del comando, el siguiente mensaje será mostrado en el terminal:</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="pygments highlight"><code>:ldap-spring-boot-samples/odm:test
Listening for transport dt_socket at address: 5005
&gt; Building 93% &gt; :ldap-spring-boot-samples/odm:test &gt; 0 tests completed</code></pre>
</div>
</div>
<div class="paragraph">
<p>El comando ejecutado abrirá el puerto <strong>5005</strong> y el proceso quedará en espera hasta que el IDE se conecte. En mi caso uso IntelliJ, clic en las siguientes opciones <code>Run &gt; Attach to Local Process</code> y seleccionar el proceso Java que usa el puerto <strong>5005</strong> ó <code>Run &gt; Debug…&#8203; &gt; Edit Configurations…&#8203; &gt; Add New Configuration &gt; Remote</code>, verificar el <code>Host</code> y <code>Port</code> y clic en <code>Debug</code>.</p>
</div>
<div class="paragraph">
<p>Otra forma de habilitar el debug en los test es hacer uso del parámetro <code>jvmArgs</code> como se muestra a continuación:</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="pygments highlight"><code>test {
    jvmArgs '-Xdebug', '-Xrunjdwp:transport=dt_socket,server=y,suspend=y,address=5006'
}</code></pre>
</div>
</div>
<div class="paragraph">
<p><strong>NOTA:</strong> el puerto ha cambiado a <strong>5006</strong>.</p>
</div>
<div class="paragraph">
<p>En este caso, no hacemos uso de <code>-Dtest.debug</code>, ejecutaremos la tarea <code>test</code> del subproyecto directamente:</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="pygments highlight"><code data-lang="bash">./gradlew :ldap-spring-boot-samples/odm:test</code></pre>
</div>
</div>
<div class="paragraph">
<p>El siguiente mensaje se muestra:</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="pygments highlight"><code>:ldap-spring-boot-samples/odm:test
Listening for transport dt_socket at address: 5006
&gt; Building 93% &gt; :ldap-spring-boot-samples/odm:test &gt; 0 tests completed</code></pre>
</div>
</div>
<div class="paragraph">
<p>Como podemos observar ahora el puesto es <strong>5006</strong>, para conectarnos seguimos los pasos antes mencionados.</p>
</div>
