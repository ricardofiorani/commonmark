# Change Log
All notable changes to this project will be documented in this file.
Updates should follow the [Keep a CHANGELOG](https://keepachangelog.com/) principles.

## [Unreleased][unreleased]

### Added

 - Added a new `Html5EntityDecoder` class (#387)

### Changed

 - Made entity decoding less memory-intensive (#386)

### Fixed

 - Fixed PHP 7.4 compatibility issue

### Deprecated

 - Deprecated the `Html5Entities` class - use `Html5EntityDecoder` instead (#387)

## [1.0.0] - 2019-06-29

No changes were made since 1.0.0-rc1.

## [1.0.0-rc1] - 2019-06-19

### Added

 - Extracted a `ReferenceMapInterface` from the `ReferenceMap` class
 - Added optional `ReferenceMapInterface` parameter to the `Document` constructor

### Changed

 - Replaced all references to `ReferenceMap` with `ReferenceMapInterface`
 - `ReferenceMap::addReference()` no longer returns `$this`

### Fixed

 - Fixed bug where elements with content of `"0"` wouldn't be rendered (#376)

## [1.0.0-beta4] - 2019-06-05

### Added

 - Added event dispatcher functionality (#359, #372)

### Removed

 - Removed `DocumentProcessorInterface` functionality in favor of event dispatching (#373)

## [1.0.0-beta3] - 2019-05-27

### Changed

 - Made the `Delimiter` class final and extracted a new `DelimiterInterface`
   - Modified most external usages to use this new interface
 - Renamed three `Delimiter` methods:
   - `getOrigDelims()` renamed to `getOriginalLength()`
   - `getNumDelims()` renamed to `getLength()`
   - `setNumDelims()` renamed to `setLength()`
 - Made additional classes final:
   - `DelimiterStack`
   - `ReferenceMap`
   - `ReferenceParser`
 - Moved `ReferenceParser` into the `Reference` sub-namespace

### Removed

 - Removed unused `Delimiter` methods:
   - `setCanOpen()`
   - `setCanClose()`
   - `setChar()`
   - `setIndex()`
   - `setInlineNode()`
 - Removed fluent interface from `Delimiter` (setter methods now have no return values)

## [1.0.0-beta2] - 2019-05-27

### Changed

 - `DelimiterProcessorInterface::process()` will accept any type of `AbstractStringContainer` now, not just `Text` nodes
 - The `Delimiter` constructor, `getInlineNode()`, and `setInlineNode()` no longer accept generic `Node` elements - only `AbstractStringContainer`s


### Removed

 - Removed all deprecated functionality:
   - The `safe` option (use `html_input` and `allow_unsafe_links` options instead)
   - All deprecated `RegexHelper` constants
   - `DocParser::getEnvironment()` (you should obtain it some other way)
   - `AbstractInlineContainer` (use `AbstractInline` instead and make `isContainer()` return `true`)

## [1.0.0-beta1] - 2019-05-26

### Added

 - Added proper support for delimiters, including custom delimiters
   - `addDelimiterProcessor()` added to `ConfigurableEnvironmentInterface` and `Environment`
 - Basic delimiters no longer need custom parsers - they'll be parsed automatically
 - Added new methods:
   - `AdjacentTextMerger::mergeTextNodesBetweenExclusive()`
   - `CommonMarkConveter::getEnvironment()`
   - `Configuration::set()`
 - Extracted some new interfaces from base classes:
   - `DocParserInterface` created from `DocParser`
   - `ConfigurationInterface` created from `Configuration`
   - `ReferenceInterface` created from `Reference`

### Changed

 - Renamed several methods of the `Configuration` class:
   - `getConfig()` renamed to `get()`
   - `mergeConfig()` renamed to `merge()`
   - `setConfig()` renamed to `replace()`
 - Changed `ConfigurationAwareInterface::setConfiguration()` to accept the new `ConfigurationInterface` instead of the concrete class
 - Renamed the `AdjoiningTextCollapser` class to `AdjacentTextMerger`
   - Replaced its `collapseTextNodes()` method with the new `mergeChildNodes()` method
 - Made several classes `final`:
   - `Configuration`
   - `DocParser`
   - `HtmlRenderer`
   - `InlineParserEngine`
   - `NodeWalker`
   - `Reference`
   - All of the block/inline parsers and renderers
 - Reduced visibility of several internal methods to `private`:
    - `DelimiterStack::findEarliest()`
    - All `protected` methods in `InlineParserEngine`
 - Marked some classes and methods as `@internal`
 - `ElementRendererInterface` now requires a public `renderInline()` method; added this to `HtmlRenderer`
 - Changed `InlineParserEngine::parse()` to require an `AbstractStringContainerBlock` instead of the generic `Node` class
 - Un-deprecated the `CommonmarkConverter::VERSION` constant
 - The `Converter` constructor now requires an instance of `DocParserInterface` instead of the concrete `DocParser`
 - Changed `Emphasis`, `Strong`, and `AbstractWebResource` to directly extend `AbstractInline` instead of the (now-deprecated) intermediary `AbstractInlineContainer` class

### Fixed

 - Fixed null errors when inserting sibling `Node`s without parents
 - Fixed `NodeWalkerEvent` not requiring a `Node` via its constructor
 - Fixed `Reference::normalizeReference()` improperly converting to uppercase instead of performing proper Unicode case-folding
 - Fixed strong emphasis delimiters not being preserved when `enable_strong` is set to `false` (it now works identically to `enable_em`)

### Deprecated

 - Deprecated `DocParser::getEnvironment()` (you should obtain it some other way)
 - Deprecated `AbstractInlineContainer` (use `AbstractInline` instead and make `isContainer()` return `true`)

### Removed

 - Removed inline processor functionality now that we have proper delimiter support:
   - Removed `addInlineProcessor()` from `ConfigurableEnvironmentInterface` and `Environment`
   - Removed `getInlineProcessors()` from `EnvironmentInterface` and `Environment`
   - Removed `EmphasisProcessor`
   - Removed `InlineProcessorInterface`
 - Removed `EmphasisParser` now that we have proper delimiter support
 - Removed support for non-UTF-8-compatible encodings
    - Removed `getEncoding()` from `ContextInterface`
    - Removed `getEncoding()`, `setEncoding()`, and `$encoding` from `Context`
    - Removed `getEncoding()` and the second `$encoding` constructor param from `Cursor`
 - Removed now-unused methods
   - Removed `DelimiterStack::getTop()` (no replacement)
   - Removed `DelimiterStack::iterateByCharacters()` (use the new `processDelimiters()` method instead)
   - Removed the protected `DelimiterStack::findMatchingOpener()` method

[unreleased]: https://github.com/thephpleague/commonmark/compare/1.0.0...HEAD
[1.0.0]: https://github.com/thephpleague/commonmark/compare/1.0.0-rc1...1.0.0
[1.0.0-rc1]: https://github.com/thephpleague/commonmark/compare/1.0.0-beta4...1.0.0-rc1
[1.0.0-beta4]: https://github.com/thephpleague/commonmark/compare/1.0.0-beta3...1.0.0-beta4
[1.0.0-beta3]: https://github.com/thephpleague/commonmark/compare/1.0.0-beta2...1.0.0-beta3
[1.0.0-beta2]: https://github.com/thephpleague/commonmark/compare/1.0.0-beta1...1.0.0-beta2
[1.0.0-beta1]: https://github.com/thephpleague/commonmark/compare/0.19.2...1.0.0-beta1
