UPGRADE FROM 3.x to 4.0
=======================

ClassLoader
-----------

 * The component has been removed. Use Composer instead.

Console
-------

 * Setting unknown style options is not supported anymore and throws an
   exception.

Debug
-----


 * The `ContextErrorException` class has been removed. Use `\ErrorException` instead.

 * `FlattenException::getTrace()` now returns additional type descriptions
   `integer` and `float`.

DependencyInjection
-------------------

 * Autowiring-types have been removed, use aliases instead.

   Before:

   ```xml
   <service id="annotations.reader" class="Doctrine\Common\Annotations\AnnotationReader" public="false">
       <autowiring-type>Doctrine\Common\Annotations\Reader</autowiring-type>
   </service>
   ```

   After:

   ```xml
   <service id="annotations.reader" class="Doctrine\Common\Annotations\AnnotationReader" public="false" />
   <service id="Doctrine\Common\Annotations\Reader" alias="annotations.reader" public="false" />
   ```

 * Service identifiers are now case sensitive.

 * The `Reference` and `Alias` classes do not make service identifiers lowercase anymore.

 * Using the `PhpDumper` with an uncompiled `ContainerBuilder` is not supported
   anymore.

 * Extending the containers generated by `PhpDumper` is not supported
   anymore.

 * The `DefinitionDecorator` class has been removed. Use the `ChildDefinition`
   class instead.

 * Using unsupported configuration keys in YAML configuration files raises an
   exception.

 * Using unsupported options to configure service aliases raises an exception.

 * Setting or unsetting a private service with the `Container::set()` method is
   no longer supported. Only public services can be set or unset.

 * Checking the existence of a private service with the `Container::has()`
   method is no longer supported and will return `false`.

 * Requesting a private service with the `Container::get()` method is no longer
   supported.

 * The ``strict`` attribute in service arguments has been removed.
   The attribute is ignored since 3.0, so you can simply remove it.

EventDispatcher
---------------

 * The `ContainerAwareEventDispatcher` class has been removed.
   Use `EventDispatcher` with closure-proxy injection instead.

ExpressionLanguage
----------

 * The ability to pass a `ParserCacheInterface` instance to the `ExpressionLanguage`
   class has been removed. You should use the `CacheItemPoolInterface` interface
   instead.

Finder
------

 * The `ExceptionInterface` has been removed.

Form
----

 * The `choices_as_values` option of the `ChoiceType` has been removed.

 * Support for data objects that implements both `Traversable` and
   `ArrayAccess` in `ResizeFormListener::preSubmit` method has been removed.

 * Using callable strings as choice options in ChoiceType is not supported
   anymore in favor of passing PropertyPath instances.

   Before:

   ```php
   'choice_value' => new PropertyPath('range'),
   'choice_label' => 'strtoupper',
   ```

   After:

   ```php
   'choice_value' => 'range',
   'choice_label' => function ($choice) {
       return strtoupper($choice);
   },
   ```

 * Caching of the loaded `ChoiceListInterface` in the `LazyChoiceList` has been removed,
   it must be cached in the `ChoiceLoaderInterface` implementation instead.

 * Calling `isValid()` on a `Form` instance before submitting it is not supported
   anymore and raises an exception.

   Before:

   ```php
   if ($form->isValid()) {
       // ...
   }
   ```

   After:

   ```php
   if ($form->isSubmitted() && $form->isValid()) {
       // ...
   }
   ```

FrameworkBundle
---------------

 * Support for absolute template paths has been removed.

 * The following form types registered as services have been removed; use their
   fully-qualified class name instead:

    - `"form.type.birthday"`
    - `"form.type.checkbox"`
    - `"form.type.collection"`
    - `"form.type.country"`
    - `"form.type.currency"`
    - `"form.type.date"`
    - `"form.type.datetime"`
    - `"form.type.email"`
    - `"form.type.file"`
    - `"form.type.hidden"`
    - `"form.type.integer"`
    - `"form.type.language"`
    - `"form.type.locale"`
    - `"form.type.money"`
    - `"form.type.number"`
    - `"form.type.password"`
    - `"form.type.percent"`
    - `"form.type.radio"`
    - `"form.type.range"`
    - `"form.type.repeated"`
    - `"form.type.search"`
    - `"form.type.textarea"`
    - `"form.type.text"`
    - `"form.type.time"`
    - `"form.type.timezone"`
    - `"form.type.url"`
    - `"form.type.button"`
    - `"form.type.submit"`
    - `"form.type.reset"`

 * The `framework.serializer.cache` option and the services
   `serializer.mapping.cache.apc` and `serializer.mapping.cache.doctrine.apc`
   have been removed. APCu should now be automatically used when available.

 * The `Symfony\Bundle\FrameworkBundle\DependencyInjection\Compiler\AddConsoleCommandPass` has been removed. Use `Symfony\Component\Console\DependencyInjection\AddConsoleCommandPass` instead.

 * The `Symfony\Bundle\FrameworkBundle\DependencyInjection\Compiler\SerializerPass` class has been removed.
   Use the `Symfony\Component\Serializer\DependencyInjection\SerializerPass` class instead.

 * The `Symfony\Bundle\FrameworkBundle\DependencyInjection\Compiler\FormPass` class has been
   removed. Use the `Symfony\Component\Form\DependencyInjection\FormPass` class instead.

SecurityBundle
--------------

 * The `FirewallContext::getContext()` method has been removed, use the `getListeners()` method instead.

 * The `FirewallMap::$map` and `$container` properties have been removed.

HttpFoundation
---------------

 * Extending the following methods of `Response`
   is no longer possible (these methods are now `final`):

    - `setDate`/`getDate`
    - `setExpires`/`getExpires`
    - `setLastModified`/`getLastModified`
    - `setProtocolVersion`/`getProtocolVersion`
    - `setStatusCode`/`getStatusCode`
    - `setCharset`/`getCharset`
    - `setPrivate`/`setPublic`
    - `getAge`
    - `getMaxAge`/`setMaxAge`
    - `setSharedMaxAge`
    - `getTtl`/`setTtl`
    - `setClientTtl`
    - `getEtag`/`setEtag`
    - `hasVary`/`getVary`/`setVary`
    - `isInvalid`/`isSuccessful`/`isRedirection`/`isClientError`/`isServerError`
    - `isOk`/`isForbidden`/`isNotFound`/`isRedirect`/`isEmpty`

 * The ability to check only for cacheable HTTP methods using `Request::isMethodSafe()` is
   not supported anymore, use `Request::isMethodCacheable()` instead.

HttpKernel
----------

 * Possibility to pass non-scalar values as URI attributes to the ESI and SSI
   renderers has been removed. The inline fragment renderer should be used with
   non-scalar attributes.

 * The `ControllerResolver::getArguments()` method has been removed. If you
   have your own `ControllerResolverInterface` implementation, you should
   inject an `ArgumentResolverInterface` instance.

 * The `DataCollector::varToString()` method has been removed in favor of `cloneVar()`.

 * The `Psr6CacheClearer::addPool()` method has been removed. Pass an array of pools indexed
   by name to the constructor instead.

Process
-------

 * The `ProcessUtils::escapeArgument()` method has been removed, use a command line array or give env vars to the `Process::start/run()` method instead.

 * Environment variables are always inherited in sub-processes.

 * Configuring `proc_open()` options has been removed.

 * Configuring Windows and sigchild compatibility is not possible anymore - they are always enabled.

 * Extending `Process::run()`, `Process::mustRun()` and `Process::restart()` is
   not supported anymore.

Security
--------

 * The `RoleInterface` has been removed. Extend the `Symfony\Component\Security\Core\Role\Role`
   class instead.

Serializer
----------

 * The ability to pass a Doctrine `Cache` instance to the `ClassMetadataFactory`
   class has been removed. You should use the `CacheClassMetadataFactory` class
   instead.

 * Not defining the 6th argument `$format = null` of the
   `AbstractNormalizer::instantiateObject()` method when overriding it is not
   supported anymore.

 * Extending `ChainDecoder`, `ChainEncoder`, `ArrayDenormalizer` is not supported
   anymore.

Translation
-----------

 * Removed the backup feature from the file dumper classes.

TwigBridge
----------

 * Removed the possibility to inject the Form `TwigRenderer` into the `FormExtension`.
   Upgrade Twig to `^1.30`, inject the `Twig_Environment` into the `TwigRendererEngine` and load
   the `TwigRenderer` using the `Twig_FactoryRuntimeLoader` instead.

   Before:

   ```php
   use Symfony\Bridge\Twig\Extension\FormExtension;
   use Symfony\Bridge\Twig\Form\TwigRenderer;
   use Symfony\Bridge\Twig\Form\TwigRendererEngine;

   // ...
   $rendererEngine = new TwigRendererEngine(array('form_div_layout.html.twig'));
   $rendererEngine->setEnvironment($twig);
   $twig->addExtension(new FormExtension(new TwigRenderer($rendererEngine, $csrfTokenManager)));
   ```

   After:

   ```php
   $rendererEngine = new TwigRendererEngine(array('form_div_layout.html.twig'), $twig);
   $twig->addRuntimeLoader(new \Twig_FactoryRuntimeLoader(array(
       TwigRenderer::class => function () use ($rendererEngine, $csrfTokenManager) {
           return new TwigRenderer($rendererEngine, $csrfTokenManager);
       },
   )));
   $twig->addExtension(new FormExtension());
   ```

 * Removed the `TwigRendererEngineInterface` interface.

 * The `TwigRendererEngine::setEnvironment()` method has been removed.
   Pass the Twig Environment as second argument of the constructor instead.

Validator
---------

 * The `DateTimeValidator::PATTERN` constant was removed.

 * `Tests\Constraints\AbstractConstraintValidatorTest` has been removed in
   favor of `Test\ConstraintValidatorTestCase`.

   Before:

   ```php
   // ...
   use Symfony\Component\Validator\Tests\Constraints\AbstractConstraintValidatorTest;

   class MyCustomValidatorTest extends AbstractConstraintValidatorTest
   {
       // ...
   }
   ```

   After:

   ```php
   // ...
   use Symfony\Component\Validator\Test\ConstraintValidatorTestCase;

   class MyCustomValidatorTest extends ConstraintValidatorTestCase
   {
       // ...
   }
   ```

 * The default value of the strict option of the `Choice` Constraint has been
   changed to `true` as of 4.0. If you need the previous behaviour ensure to
   set the option to `false`.

Yaml
----

 * Omitting the key of a mapping is not supported anymore and throws a `ParseException`.

 * Mappings with a colon (`:`) that is not followed by a whitespace are not
   supported anymore and lead to a `ParseException`(e.g. `foo:bar` must be
   `foo: bar`).

 * Starting an unquoted string with `%` leads to a `ParseException`.

 * The `Dumper::setIndentation()` method was removed. Pass the indentation
   level to the constructor instead.

 * Removed support for passing `true`/`false` as the second argument to the
   `parse()` method to trigger exceptions when an invalid type was passed.

   Before:

   ```php
   Yaml::parse('{ "foo": "bar", "fiz": "cat" }', true);
   ```

   After:

   ```php
   Yaml::parse('{ "foo": "bar", "fiz": "cat" }', Yaml::PARSE_EXCEPTION_ON_INVALID_TYPE);
   ```

 * Removed support for passing `true`/`false` as the third argument to the
   `parse()` method to toggle object support.

   Before:

   ```php
   Yaml::parse('{ "foo": "bar", "fiz": "cat" }', false, true);
   ```

   After:

   ```php
   Yaml::parse('{ "foo": "bar", "fiz": "cat" }', Yaml::PARSE_OBJECT);
   ```

 * Removed support for passing `true`/`false` as the fourth argument to the
   `parse()` method to parse objects as maps.

   Before:

   ```php
   Yaml::parse('{ "foo": "bar", "fiz": "cat" }', false, false, true);
   ```

   After:

   ```php
   Yaml::parse('{ "foo": "bar", "fiz": "cat" }', Yaml::PARSE_OBJECT_FOR_MAP);
   ```

 * Removed support for passing `true`/`false` as the fourth argument to the
   `dump()` method to trigger exceptions when an invalid type was passed.

   Before:

   ```php
   Yaml::dump(array('foo' => new A(), 'bar' => 1), 0, 0, true);
   ```

   After:

   ```php
   Yaml::dump(array('foo' => new A(), 'bar' => 1), 0, 0, Yaml::DUMP_EXCEPTION_ON_INVALID_TYPE);
   ```

 * Removed support for passing `true`/`false` as the fifth argument to the
   `dump()` method to toggle object support.

   Before:

   ```php
   Yaml::dump(array('foo' => new A(), 'bar' => 1), 0, 0, false, true);
   ```

   After:

   ```php
   Yaml::dump(array('foo' => new A(), 'bar' => 1), 0, 0, false, Yaml::DUMP_OBJECT);
   ```

 * The `!!php/object` tag to indicate dumped PHP objects was removed in favor of
   the `!php/object` tag.

 * Duplicate mapping keys lead to a `ParseException`.

 * The constructor arguments `$offset`, `$totalNumberOfLines` and
   `$skippedLineNumbers` of the `Parser` class were removed.

Ldap
----

 * The `RenameEntryInterface` has been deprecated, and merged with `EntryManagerInterface`

SecurityBundle
--------------

 * The `UserPasswordEncoderCommand` class does not allow `null` as the first argument anymore.
 
 * `UserPasswordEncoderCommand` does not implement `ContainerAwareInterface` anymore.

Workflow
--------

 * Removed class name support in `WorkflowRegistry::add()` as second parameter.
