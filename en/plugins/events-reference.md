# Events Reference

{intro} Craft provides several events that alert plugins when things are happening.

> {tip} See [Hooks and Events](/v2/plugins/hooks-and-events.html) for an explanation of how events work in Craft, and how they differ from hooks.

## General Events

### onEditionChange

Raised by
: [AppBehavior::setEdition()](/api/v2/etc/behaviors/AppBehavior.html#setEdition-detail)

Raised after Craft’s edition changes.

#### Params:

*   _edition_ – The new edition (`0` for Personal, `1` for Client, `2` for Pro)

### db.onBackup

Raised by
: [DbConnection::backup()](/api/v2/etc/db/DbConnection.html#backup-detail)

Raised after a database backup has been created.

#### Params:

*   _filePath_ – The path to the new backup file.

### email.onBeforeSendEmail

Raised by
: [EmailService::sendEmail()](/api/v2/services/EmailService.html#sendEmail-detail), [EmailService::sendEmailByKey()](/api/v2/services/EmailService.html#sendEmailByKey-detail)

Raised right before an email is sent.

#### Params:

*   _user_ – A [UserModel](/api/v2/models/UserModel.html) object representing the user that is receiving the email.
*   _emailModel_ – The [EmailModel](/api/v2/models/EmailModel.html) defining the email to be sent.
*   _variables_ – Any variables that are going to be available to the email template.

> {tip} Event handlers can prevent the email from getting sent by setting [`$event->performAction`](/api/v2/etc/events/Event.html#performAction-detail) to `false`.

### email.onSendEmail

Raised by
: [EmailService::sendEmail()](/api/v2/services/EmailService.html#sendEmail-detail), [EmailService::sendEmailByKey()](/api/v2/services/EmailService.html#sendEmailByKey-detail)

Raised when an email is sent.

#### Params:

*   _user_ – A [UserModel](/api/v2/models/UserModel.html) object representing the user that received the email.
*   _emailModel_ – The [EmailModel](/api/v2/models/EmailModel.html) defining the email that was sent.
*   _variables_ – Any variables that were available to the email template.

### email.onSendEmailError

Raised by
: [EmailService::sendEmail()](/api/v2/services/EmailService.html#sendEmail-detail), [EmailService::sendEmailByKey()](/api/v2/services/EmailService.html#sendEmailByKey-detail)

Raised when an email fails to send.

#### Params:

*   _user_ – A [UserModel](/api/v2/models/UserModel.html) object representing the user that received the email.
*   _emailModel_ – The [EmailModel](/api/v2/models/EmailModel.html) defining the email that was sent.
*   _variables_ – Any variables that were available to the email template.
*   _error_ – The error defined by PHPMailer.

### i18n.onAddLocale

Raised by
: [LocalizationService::addSiteLocale()](/api/v2/services/LocalizationService.html#addSiteLocale-detail)

Raised when a new locale is added to the site.

#### Params:

*   _localeId_ – The ID of the locale that was just added.

### i18n.onBeforeDeleteLocale

Raised by
: [LocalizationService::deleteSiteLocale()](/api/v2/services/LocalizationService.html#deleteSiteLocale-detail)

Raised right before a locale is deleted.

#### Params:

*   _localeId_ – The ID of the locale that’s about to be deleted.
*   _transferContentTo_ – The ID of the locale that the deleted locale’s content should be transferred to, if any.

> {tip} Event handlers can prevent the locale from getting deleted by setting [`$event->performAction`](/api/v2/etc/events/Event.html#performAction-detail) to `false`.

### localization.onDeleteLocale

Raised by
: [LocalizationService::deleteSiteLocale()](/api/v2/services/LocalizationService.html#deleteSiteLocale-detail)

Raised when a locale is deleted.

#### Params:

*   _localeId_ – The ID of the locale that was deleted.
*   _transferContentTo_ – The ID of the locale that the deleted locale’s content should have be transferred to, if any.

### plugins.onLoadPlugins

Raised by
: [PluginsService::loadPlugins()](/api/v2/services/PluginsService.html#loadPlugins-detail)

Raised when Craft has finished loading all the plugins.

### updates.onBeginUpdate

Raised by
: [UpdatesService::prepareUpdate()](/api/v2/services/UpdatesService.html#prepareUpdate-detail)

Raised when an update is beginning.

#### Params:

*   _type_ – A string either set to 'auto' or 'manual' indicating if the update is a manual update or auto-update.

### updates.onEndUpdate

Raised by
: [UpdatesService::updateCleanUp()](/api/v2/services/UpdatesService.html#updateCleanUp-detail)

Raised when an update has ended.

#### Params:

*   _success_ – Set to true or false indicating if the update was successful or not.

## Element API Events

### content.onSaveContent

Raised by
: [ContentService::saveContent()](/api/v2/services/ContentService.html#saveContent-detail)

Raised when any element’s content is saved.

#### Params:

*   _content_ – A [ContentModel](/api/v2/models/ContentModel.html) object representing the saved content.
*   _isNewContent_ – A boolean indicating whether this was a new set of content.

### elements.onBeforeBuildElementsQuery

Raised by
: [ElementsService::buildElementsQuery()](/api/v2/services/ElementsService.html#buildElementsQuery-detail)

Raised before Craft builds out an elements query, enabling plugins to modify the query or prevent it from actually happening.

#### Params:

*   _criteria_ – The [ElementCriteriaModel](/api/v2/models/ElementCriteriaModel.html) object that defines the parameters for the query.
*   _justIds_ – `true` or `false` depending on whether the query should only be returning the IDs of the matched elements.
*   _query_ – The [DbCommand](/api/v2/etc/db/DbCommand.html) object that is being built out.

> {tip} Event handlers can prevent the element query from being executed by setting [`$event->performAction`](/api/v2/etc/events/Event.html#performAction-detail) to `false`.

### elements.onBuildElementsQuery

Raised by
: [ElementsService::buildElementsQuery()](/api/v2/services/ElementsService.html#buildElementsQuery-detail)

Raised after Craft has built out an elements query, enabling plugins to modify the query.

#### Params:

*   _criteria_ – The [ElementCriteriaModel](/api/v2/models/ElementCriteriaModel.html) object that defines the parameters for the query.
*   _justIds_ – `true` or `false` depending on whether the query should only be returning the IDs of the matched elements.
*   _query_ – The [DbCommand](/api/v2/etc/db/DbCommand.html) object that is being built out.

### elements.onBeforeDeleteElements

Raised by
: [ElementsService::deleteElementById()](/api/v2/services/ElementsService.html#deleteElementById-detail)

Raised right before any elements are about to be deleted.

#### Params:

*   _elementIds_ – The element IDs that are about to be deleted.

### elements.onMergeElements

Raised by
: [ElementsService::mergeElementsByIds()](/api/v2/services/ElementsService.html#mergeElementsByIds-detail)

Raised when any element merged with another element.

#### Params:

*   _mergedElementId_ – The id of the element being merged.
*   _prevailingElementId_ – The id of the element that prevailed in the merge.

### structures.onBeforeMoveElement

Raised by
: [StructuresService::prepend()](/api/v2/services/StructuresService.html#prepend-detail), [StructuresService::append()](/api/v2/services/StructuresService.html#append-detail), [StructuresService::prependToRoot()](/api/v2/services/StructuresService.html#prependToRoot-detail), [StructuresService::appendToRoot()](/api/v2/services/StructuresService.html#appendToRoot-detail), [StructuresService::moveBefore()](/api/v2/services/StructuresService.html#moveBefore-detail), [StructuresService::moveAfter()](/api/v2/services/StructuresService.html#moveAfter-detail)

Raised right before an element is moved within a structure.

#### Params:

*   _structureId_ – The ID of the structure that the element is being moved within.
*   _element_ – A [BaseElementModel](/api/v2/models/BaseElementModel.html) object representing the element that is about to be moved.

> {tip} Event handlers can prevent the element from getting moved by setting [`$event->performAction`](/api/v2/etc/events/Event.html#performAction-detail) to `false`.

### structures.onMoveElement

Raised by
: [StructuresService::prepend()](/api/v2/services/StructuresService.html#prepend-detail), [StructuresService::append()](/api/v2/services/StructuresService.html#append-detail), [StructuresService::prependToRoot()](/api/v2/services/StructuresService.html#prependToRoot-detail), [StructuresService::appendToRoot()](/api/v2/services/StructuresService.html#appendToRoot-detail), [StructuresService::moveBefore()](/api/v2/services/StructuresService.html#moveBefore-detail), [StructuresService::moveAfter()](/api/v2/services/StructuresService.html#moveAfter-detail)

Raised when an element is moved within a structure.

#### Params:

*   _structureId_ – The ID of the structure that the element was moved within.
*   _element_ – A [BaseElementModel](/api/v2/models/BaseElementModel.html) object representing the element that was moved.

### elements.onBeforePerformAction

Raised by
: [ElementIndexController::actionPerformAction()](/api/v2/controllers/ElementIndexController.html#actionPerformAction-detail)

Raised before a batch element action gets triggered.

#### Params:

*   _action_ – The element action class that is going to be performing the action.
*   _criteria_ – The [ElementCriteriaModel](/api/v2/models/ElementCriteriaModel.html) object that defines which element(s.html) the user has chosen to perform the action on.

> {tip} Event handlers can prevent the element action from being triggered by setting [`$event->performAction`](/api/v2/etc/events/Event.html#performAction-detail) to `false`.

### elements.onPerformAction

Raised by
: [ElementIndexController::actionPerformAction()](/api/v2/controllers/ElementIndexController.html#actionPerformAction-detail)

Raised after a batch element action has been performed.

#### Params:

*   _action_ – The element action class that performed the action.
*   _criteria_ – The [ElementCriteriaModel](/api/v2/models/ElementCriteriaModel.html) object that defines which element(s.html) the user had chosen to perform the action on.

### elements.onPopulateElement

Raised by
: [ElementsService::findElements()](/api/v2/services/ElementsService.html#findElements-detail)

Raised when any element model is populated from its database result.

#### Params:

*   _element_ – The populated element.
*   _result_ – The raw data representing the element from the database.

### elements.onPopulateElements

Raised by
: [ElementsService::populateElements()](/api/v2/services/ElementsService.html#populateElements-detail)

Raised when all of the element models have been populated from an element query.

#### Params:

*   _elements_ – An array of the populated elements.
*   _criteria_ – The ElementCriteriaModel that was used to define the element query.

### elements.onBeforeSaveElement

Raised by
: [ElementsService::saveElement()](/api/v2/services/ElementsService.html#saveElement-detail)

Raised right before an element is saved.

#### Params:

*   _element_ – A [BaseElementModel](/api/v2/models/BaseElementModel.html) object representing the element that is about to be saved.
*   _isNewElement_ – A boolean indicating whether this is a brand new element.

> {tip} Event handlers can prevent the element from getting saved by setting [`$event->performAction`](/api/v2/etc/events/Event.html#performAction-detail) to `false`.

### elements.onSaveElement

Raised by
: [ElementsService::saveElement()](/api/v2/services/ElementsService.html#saveElement-detail)

Raised when an element is saved.

#### Params:

*   _element_ – A [BaseElementModel](/api/v2/models/BaseElementModel.html) object representing the element that was just saved.
*   _isNewElement_ – A boolean indicating whether this is a brand new element.

### fields.onSaveFieldLayout

Raised by
: [FieldsService::saveLayout()](/api/v2/services/FieldsService.html#saveLayout-detail)

Raised when a field layout is saved.

#### Params:

*   _layout_ – A [FieldLayoutModel](/api/v2/models/FieldLayoutModel.html) object representing the field layout that was just saved.

## Entry API Events

### entries.onBeforeSaveEntry

Raised by
: [EntriesService::saveEntry()](/api/v2/services/EntriesService.html#saveEntry-detail)

Raised right before an entry is saved.

#### Params:

*   _entry_ – An [EntryModel](/api/v2/models/EntryModel.html) object representing the entry that is about to be saved.
*   _isNewEntry_ – A boolean indicating whether this is a brand new entry.

> {tip} Event handlers can prevent the entry from getting saved by setting [`$event->performAction`](/api/v2/etc/events/Event.html#performAction-detail) to `false`.

### entries.onSaveEntry

Raised by
: [EntriesService::saveEntry()](/api/v2/services/EntriesService.html#saveEntry-detail)

Raised when an entry is saved.

#### Params:

*   _entry_ – An [EntryModel](/api/v2/models/EntryModel.html) object representing the entry that was just saved.
*   _isNewEntry_ – A boolean indicating whether this is a brand new entry.

### entries.onBeforeDeleteEntry

Raised by
: [EntriesService::deleteEntry()](/api/v2/services/EntriesService.html#deleteEntry-detail)

Raised right before an entry is deleted.

#### Params:

*   _entry_ – An [EntryModel](/api/v2/models/EntryModel.html) object representing the entry that is about to be deleted.

> {tip} Event handlers can prevent the entry from being deleted by setting [`$event->performAction`](/api/v2/etc/events/Event.html#performAction-detail) to `false`.

### entries.onDeleteEntry

Raised by
: [EntriesService::deleteEntry()](/api/v2/services/EntriesService.html#deleteEntry-detail)

Raised when an entry is deleted.

#### Params:

*   _entry_ – An [EntryModel](/api/v2/models/EntryModel.html) object representing the entry that was just deleted.

### entryRevisions.onSaveDraft

Raised by
: [EntryRevisionsService::saveDraft()](/api/v2/services/EntryRevisionsService.html#saveDraft-detail)

Raised right before a draft is saved.

#### Params:

*   _draft_ – An [EntryDraftModel](/api/v2/models/EntryDraftModel.html) object representing the draft that was just saved.
*   _isNewDraft_ – A boolean indicating whether this is a brand new draft.

### entryRevisions.onPublishDraft

Raised by
: [EntryRevisionsService::publishDraft()](/api/v2/services/EntryRevisionsService.html#publishDraft-detail)

Raised when an draft is published.

#### Params:

*   _draft_ – An [EntryDraftModel](/api/v2/models/EntryDraftModel.html) object representing the draft that was just published.

### entryRevisions.onBeforeDeleteDraft

Raised by
: [EntryRevisionsService::deleteDraft()](/api/v2/services/EntryRevisionsService.html#deleteDraft-detail)

Raised right before a draft is deleted.

#### Params:

*   _draft_ – An [EntryDraftModel](/api/v2/models/EntryDraftModel.html) object representing the draft that is able to be deleted.

> {tip} Event handlers can prevent the draft from getting deleted by setting [`$event->performAction`](/api/v2/etc/events/Event.html#performAction-detail) to `false`.

### entryRevisions.onDeleteDraft

Raised by
: [EntryRevisionsService::deleteDraft()](/api/v2/services/EntryRevisionsService.html#deleteDraft-detail)

Raised right after a draft is deleted.

#### Params:

*   _draft_ – An [EntryDraftModel](/api/v2/models/EntryDraftModel.html) object representing the draft that was just deleted.

### sections.onBeforeDeleteSection

Raised by
: [SectionsService::deleteSectionById()](/api/v2/services/SectionsService.html#deleteSectionById-detail)

Raised right before a section is deleted.

#### Params:

*   _sectionId_ – The ID of the section that is about to be deleted.

> {tip} Event handlers can prevent the section from being deleted by setting [`$event->performAction`](/api/v2/etc/events/Event.html#performAction-detail) to `false`.

### sections.onDeleteSection

Raised by
: [SectionsService::deleteSectionById()](/api/v2/services/SectionsService.html#deleteSectionById-detail)

Raised after a section is deleted.

#### Params:

*   _sectionId_ – The ID of the section that was just deleted.

### sections.onBeforeSaveEntryType

Raised by
: [SectionsService::saveEntryType()](/api/v2/services/SectionsService.html#saveEntryType-detail)

Raised right before an entry type is saved.

#### Params:

*   _entryType_ – An [EntryTypeModel](/api/v2/models/EntryTypeModel.html) object representing the entry type that is about to be saved.
*   _isNewEntryType_ – A boolean indicating whether this is a brand new entry type.

> {tip} Event handlers can prevent the entry type from getting saved by setting [`$event->performAction`](/api/v2/etc/events/Event.html#performAction-detail) to `false`.

### sections.onSaveEntryType

Raised by
: [SectionsService::saveEntryType()](/api/v2/services/SectionsService.html#saveEntryType-detail)

Raised when an entry type is saved.

#### Params:

*   _entryType_ – An [EntryTypeModel](/api/v2/models/EntryTypeModel.html) object representing the entry type that was just saved.
*   _isNewEntryType_ – A boolean indicating whether this is a brand new entry type.

### sections.onBeforeSaveSection

Raised by
: [SectionsService::saveSection()](/api/v2/services/SectionsService.html#saveSection-detail)

Raised right before a section is saved.

#### Params:

*   _section_ – An [SectionModel](/api/v2/models/SectionModel.html) object representing the section that is about to be saved.
*   _isNewSection_ – A boolean indicating whether this is a brand new section.

> {tip} Event handlers can prevent the section from getting saved by setting [`$event->performAction`](/api/v2/etc/events/Event.html#performAction-detail) to `false`.

### sections.onSaveSection

Raised by
: [SectionsService::saveSection()](/api/v2/services/SectionsService.html#saveSection-detail)

Raised when a section is saved.

#### Params:

*   _section_ – An [SectionModel](/api/v2/models/SectionModel.html) object representing the section that was just saved.
*   _isNewSection_ – A boolean indicating whether this is a brand new section.

## Category API Events

### categories.onBeforeSaveCategory

Raised by
: [CategoriesService::saveCategory()](/api/v2/services/CategoriesService.html#saveCategory-detail)

Raised before any category is saved.

#### Params:

*   _category_ – A [CategoryModel](/api/v2/models/CategoryModel.html) object representing the category about to be saved.
*   _isNewCategory_ – A boolean indicating whether this is a new category.

> {tip} Event handlers can prevent the category from getting saved by setting [`$event->performAction`](/api/v2/etc/events/Event.html#performAction-detail) to `false`.

### categories.onSaveCategory

Raised by
: [CategoriesService::saveCategory()](/api/v2/services/CategoriesService.html#saveCategory-detail)

Raised when any category is saved.

#### Params:

*   _category_ – A [CategoryModel](/api/v2/models/CategoryModel.html) object representing the category being saved.
*   _isNewCategory_ – A boolean indicating whether this is a new category.

### categories.onBeforeDeleteCategory

Raised by
: [CategoriesService::deleteCategory()](/api/v2/services/CategoriesService.html#deleteCategory-detail)

Raised before any category is deleted.

#### Params:

*   _category_ – A [CategoryModel](/api/v2/models/CategoryModel.html) object representing the category about to be deleted.

### categories.onDeleteCategory

Raised by
: [CategoriesService::deleteCategory()](/api/v2/services/CategoriesService.html#deleteCategory-detail)

Raised when any category is deleted.

#### Params:

*   _category_ – A [CategoryModel](/api/v2/models/CategoryModel.html) object representing the category being deleted.

### categories.onBeforeDeleteGroup

Raised by
: [CategoriesService::deleteGroupById()](/api/v2/services/CategoriesService.html#deleteGroupById-detail)

Raised before a category group is deleted.

#### Params:

*   _groupId_ – The ID of the category group that’s about to be deleted.

> {tip} Event handlers can prevent the category group from being deleted by setting [`$event->performAction`](/api/v2/etc/events/Event.html#performAction-detail) to `false`.

### categories.onDeleteGroup

Raised by
: [CategoriesService::deleteGroupById()](/api/v2/services/CategoriesService.html#deleteGroupById-detail)

Raised after a category group is deleted.

#### Params:

*   _groupId_ – The ID of the category group that was just deleted.

## Tag API Events

### tags.onBeforeSaveTag

Raised by
: [TagsService::saveTag()](/api/v2/services/TagsService.html#saveTag-detail)

Raised when a tag is able to be saved.

#### Params:

*   _tag_ – A [TagModel](/api/v2/models/TagModel.html) object representing the tag that is about to be saved.
*   _isNewTag_ – A boolean indicating whether this is a brand new tag.

> {tip} Event handlers can prevent the tag from getting saved by setting [`$event->performAction`](/api/v2/etc/events/Event.html#performAction-detail) to `false`.

### tags.onSaveTag

Raised by
: [TagsService::saveTag()](/api/v2/services/TagsService.html#saveTag-detail)

Raised when a tag is saved.

#### Params:

*   _tag_ – A [TagModel](/api/v2/models/TagModel.html) object representing the tag that was just saved.
*   _isNewTag_ – A boolean indicating whether this is a brand new tag.

## Asset API Events

### assets.onBeforeDeleteAsset

Raised by
: [AssetsService::deleteFiles()](/api/v2/services/AssetsService.html#deleteFiles-detail)

Raised right before an asset is deleted.

#### Params:

*   _asset_ – An [AssetFileModel](/api/v2/models/AssetFileModel.html) object representing the asset about to be deleted.

### assets.onDeleteAsset

Raised by
: [AssetsService::deleteFiles()](/api/v2/services/AssetsService.html#deleteFiles-detail)

Raised when an asset is deleted.

#### Params:

*   _asset_ – An [AssetFileModel](/api/v2/models/AssetFileModel.html) object representing the asset that was just deleted.

### assets.onBeforeReplaceFile

Raised by
: [AssetsController::actionReplaceFile()](/api/v2/controllers/AssetsController.html#actionReplaceFile-detail)

Raised right before an asset’s file is replaced.

#### Params:

*   _asset_ – An [AssetFileModel](/api/v2/models/AssetFileModel.html) object representing the asset whose file is about to be replaced.

> {tip} Event handlers can prevent the file from getting replaced by setting [`$event->performAction`](/api/v2/etc/events/Event.html#performAction-detail) to `false`.

### assets.onReplaceFile

Raised by
: [AssetsController::actionReplaceFile()](/api/v2/controllers/AssetsController.html#actionReplaceFile-detail)

Raised when any asset’s file is replaced.

#### Params:

*   _asset_ – An [AssetFileModel](/api/v2/models/AssetFileModel.html) object representing the asset whose file was just replaced.

### assets.onBeforeSaveAsset

Raised by
: [AssetsService::storeFile()](/api/v2/services/AssetsService.html#storeFile-detail)

Raised right before an asset is saved.

#### Params:

*   _asset_ – An [AssetFileModel](/api/v2/models/AssetFileModel.html) object representing the asset about to be saved.
*   _isNewAsset_ – A boolean indicating whether this is a new asset.

> {tip} Event handlers can prevent the asset from getting saved by setting [`$event->performAction`](/api/v2/etc/events/Event.html#performAction-detail) to `false`.

### assets.onSaveAsset

Raised by
: [AssetsService::storeFile()](/api/v2/services/AssetsService.html#storeFile-detail)

Raised when any asset is saved.

#### Params:

*   _asset_ – An [AssetFileModel](/api/v2/models/AssetFileModel.html) object representing the asset being saved.
*   _isNewAsset_ – A boolean indicating whether this is a new asset.

### assets.onBeforeUploadAsset

Raised by
: [BaseAssetSourceType::insertFileByPath()](/api/v2/assetsourcetypes/BaseAssetSourceType.html#insertFileByPath-detail)

Raised right before an asset is uploaded to its source.

#### Params:

*   _path_ – The path to the temporary file on the server.
*   _folder_ – An [AssetFolderModel](/api/v2/models/AssetFolderModel.html) object representing the asset folder that the file is going to be saved to.
*   _filename_ – The filename of the file.

> {tip} Event handlers can prevent the asset from getting uploaded by setting [`$event->performAction`](/api/v2/etc/events/Event.html#performAction-detail) to `false`.

## Global Set API Events

### globals.onBeforeSaveGlobalContent

Raised by
: [GlobalsService::saveContent()](/api/v2/services/GlobalsService.html#saveContent-detail)

Raised right before a Global Set’s content is saved.

#### Params:

*   _globalSet_ – A [GlobalSetModel](/api/v2/models/GlobalSetModel.html) object representing the Global Set whose content is about to be saved.

> {tip} Event handlers can prevent the global set from getting saved by setting [`$event->performAction`](/api/v2/etc/events/Event.html#performAction-detail) to `false`.

### globals.onSaveGlobalContent

Raised by
: [GlobalsService::saveContent()](/api/v2/services/GlobalsService.html#saveContent-detail)

Raised when a Global Set’s content is saved.

#### Params:

*   _globalSet_ – A [GlobalSetModel](/api/v2/models/GlobalSetModel.html) object representing the Global Set whose content was just saved.

## User API Events

### userSession.onBeforeLogin

Raised by
: [UserSessionService::login()](/api/v2/services/UserSessionService.html#login-detail)

Raised right before a user is logged in.

#### Params:

*   _username_ – A string of the username that is about to log in.

> {tip} Event handlers can prevent the user from getting logged in by setting [`$event->performAction`](/api/v2/etc/events/Event.html#performAction-detail) to `false`.

### userSession.onLogin

Raised by
: [UserSessionService::login()](/api/v2/services/UserSessionService.html#login-detail)

Raised when a user has logged in.

#### Params:

*   _username_ – A string of the username that has just logged in.

### userSession.onBeforeLogout

Raised by
: [UserSessionService::beforeLogout()](/api/v2/services/UserSessionService.html#beforeLogout-detail)

Raised right before a user is logged out.

#### Params:

*   _user_ – A [UserModel](/api/v2/models/UserModel.html) object representing the user that is getting logged out.

> {tip} Event handlers can prevent the user from getting logged out by setting [`$event->performAction`](/api/v2/etc/events/Event.html#performAction-detail) to `false`.

### userSession.onLogout

Raised by
: [UserSessionService::afterLogout()](/api/v2/services/UserSessionService.html#afterLogout-detail)

Raised when a user is logged out.

### users.onBeforeActivateUser

Raised by
: [UsersService::activateUser()](/api/v2/services/UsersService.html#activateUser-detail)

Raised right before a user is activated.

#### Params:

*   _user_ – A [UserModel](/api/v2/models/UserModel.html) object representing the user that’s about to be activated.

> {tip} Event handlers can prevent the user from getting activated by setting [`$event->performAction`](/api/v2/etc/events/Event.html#performAction-detail) to `false`.

### users.onActivateUser

Raised by
: [UsersService::activateUser()](/api/v2/services/UsersService.html#activateUser-detail)

Raised when a user is activated.

#### Params:

*   _user_ – A [UserModel](/api/v2/models/UserModel.html) object representing the user that has just been activated.

### userGroups.onBeforeAssignUserToGroups

Raised by
: [UserGroupsService::assignUserToGroups](/api/v2/services/UserGroupsService.html#assignUserToGroups-detail)

Raised right before a user’s group assignments are updated. Note that this could be called even if the group assignments haven’t changed.

#### Params:

*   _userId_ – The ID of the user whose group assignments are about to be updated.
*   _groupIds_ – The user’s new group IDs (if any).

> {tip} Event handlers can prevent the user’s new group assignments from getting saved by setting [`$event->performAction`](/api/v2/etc/events/Event.html#performAction-detail) to `false`.

### userGroups.onAssignUserToGroups

Raised by
: [UserGroupsService::assignUserToGroups](/api/v2/services/UserGroupsService.html#assignUserToGroups-detail)

Raised right after a user’s group assignments are updated.

#### Params:

*   _userId_ – The ID of the user whose group assignments were updated.
*   _groupIds_ – The user’s new group IDs (if any).

### users.onBeforeDeleteUser

Raised by
: [UsersService::deleteUser()](/api/v2/services/UsersService.html#deleteUser-detail)

Raised right before a user is deleted.

#### Params:

*   _user_ – A [UserModel](/api/v2/models/UserModel.html) object representing the user that’s about to be deleted.
*   _transferContentTo_ – A [UserModel](/api/v2/models/UserModel.html) object representing the user that the deleted user’s content should be transferred to, if any.

> {tip} Event handlers can prevent the user from getting deleted by setting [`$event->performAction`](/api/v2/etc/events/Event.html#performAction-detail) to `false`.

### users.onDeleteUser

Raised by
: [UsersService::deleteUser()](/api/v2/services/UsersService.html#deleteUser-detail)

Raised when a user is deleted.

#### Params:

*   _user_ – A [UserModel](/api/v2/models/UserModel.html) object representing the user that was just deleted.
*   _transferContentTo_ – A [UserModel](/api/v2/models/UserModel.html) object representing the user that the deleted user’s content should have been transferred to, if any.

### users.onBeforeSaveUser

Raised by
: [UsersService::saveUser()](/api/v2/services/UsersService.html#saveUser-detail)

Raised right before a user is saved.

#### Params:

*   _user_ – A [UserModel](/api/v2/models/UserModel.html) object representing the user that’s about to be saved.
*   _isNewUser_ – A boolean indicating whether this is a brand new user account.

> {tip} Event handlers can prevent the user from getting saved by setting [`$event->performAction`](/api/v2/etc/events/Event.html#performAction-detail) to `false`.

### users.onSaveUser

Raised by
: [UsersService::saveUser()](/api/v2/services/UsersService.html#saveUser-detail)

Raised when a user is saved.

#### Params:

*   _user_ – A [UserModel](/api/v2/models/UserModel.html) object representing the user that was just saved.
*   _isNewUser_ – A boolean indicating whether this is a brand new user account.

### users.onBeforeSetPassword

Raised by
: [UsersService::saveUser()](/api/v2/services/UsersService.html#saveUser-detail), [UsersService::changePassword()](/api/v2/services/UsersService.html#changePassword-detail)

Raised right before a user’s password is changed.

#### Params:

*   _user_ – A [UserModel](/api/v2/models/UserModel.html) object representing the user whose password is about to be changed.
*   **password** – The user’s new password.

> {tip} Event handlers can prevent the user’s password from getting changed by setting [`$event->performAction`](/api/v2/etc/events/Event.html#performAction-detail) to `false`.

### users.onSetPassword

Raised by
: [UsersService::saveUser()](/api/v2/services/UsersService.html#saveUser-detail), [UsersService::changePassword()](/api/v2/services/UsersService.html#changePassword-detail)

Raised when a user’s password is changed.

#### Params:

*   _user_ – A [UserModel](/api/v2/models/UserModel.html) object representing the user whose password was changed.

### users.onBeforeSuspendUser

Raised by
: [UsersService::suspendUser()](/api/v2/services/UsersService.html#suspendUser-detail)

Raised right before a user is suspended.

#### Params:

*   _user_ – A [UserModel](/api/v2/models/UserModel.html) object representing the user that’s about to be suspended.

> {tip} Event handlers can prevent the user from getting suspended by setting [`$event->performAction`](/api/v2/etc/events/Event.html#performAction-detail) to `false`.

### users.onSuspendUser

Raised by
: [UsersService::suspendUser()](/api/v2/services/UsersService.html#suspendUser-detail)

Raised when a user is suspended.

#### Params:

*   _user_ – A [UserModel](/api/v2/models/UserModel.html) object representing the user that was just suspended.

### users.onLockUser

Raised by
: [UsersService::lockUser()](/api/v2/services/UsersService.html#lockUser-detail)

Raised when a user is locked.

#### Params:

*   _user_ – A [UserModel](/api/v2/services/models/UserModel.html) object representing the user that was just locked.

### users.onBeforeUnlockUser

Raised by
: [UsersService::unlockUser()](/api/v2/services/UsersService.html#unlockUser-detail)

Raised right before a user is unlocked.

#### Params:

*   _user_ – A [UserModel](/api/v2/models/UserModel.html) object representing the user that’s about to be unlocked.

> {tip} Event handlers can prevent the user from getting unlocked by setting [`$event->performAction`](/api/v2/etc/events/Event.html#performAction-detail) to `false`.

### users.onUnlockUser

Raised by
: [UsersService::unlockUser()](/api/v2/services/UsersService.html#unlockUser-detail)

Raised when a user is unlocked.

#### Params:

*   _user_ – A [UserModel](/api/v2/models/UserModel.html) object representing the user that was just unlocked.

### users.onBeforeUnsuspendUser

Raised by
: [UsersService::unsuspendUser()](/api/v2/services/UsersService.html#unsuspendUser-detail)

Raised right before a user is unsuspended.

#### Params:

*   _user_ – A [UserModel](/api/v2/models/UserModel.html) object representing the user that’s about to be unsuspended.

> {tip} Event handlers can prevent the user from getting unsuspended by setting [`$event->performAction`](/api/v2/etc/events/Event.html#performAction-detail) to `false`.

### users.onUnsuspendUser

Raised by
: [UsersService::unsuspendUser()](/api/v2/services/UsersService.html#unsuspendUser-detail)

Raised when a user is unsuspended.

#### Params:

*   _user_ – A [UserModel](/api/v2/models/UserModel.html) object representing the user that was just unsuspended.

### users.onBeforeVerifyUser

Raised by
: [UsersController::actionSetPassword()](/api/v2/controllers/UsersController.html#actionSetPassword-detail), [UsersController::actionVerifyEmail()](/api/v2/controllers/UsersController.html#actionVerifyEmail-detail)

Raised right before a user’s email is verified.

#### Params:

*   _user_ – A [UserModel](/api/v2/models/UserModel.html) object representing the user that’s about to be verified.

### users.onVerifyUser

Raised by
: [UsersController::actionSetPassword()](/api/v2/controllers/UsersController.html#actionSetPassword-detail), [UsersController::actionVerifyEmail()](/api/v2/controllers/UsersController.html#actionVerifyEmail-detail)

Raised when a user’s email is verified.

#### Params:

*   _user_ – A [UserModel](/api/v2/models/UserModel.html) object representing the user that was just verified.

### userGroups.onBeforeAssignUserToDefaultGroup

Raised by
: [UserGroupsService::assignUserToDefaultGroup()](/api/v2/services/UserGroupsService.html#assignUserToDefaultGroup-detail)

Raised towards the end of a public user registration request before a user is assigned to a default user group.

#### Params:

*   _user_ – A [UserModel](/api/v2/models/UserModel.html) object representing the user that was just registered.
*   _defaultGroupId_ \- The id of the user group that the user is about to be assigned to.

### userGroups.onAssignUserToDefaultGroup

Raised by
: [UserGroupsService::assignUserToDefaultGroup()](/api/v2/services/UserGroupsService.html#assignUserToDefaultGroup-detail)

Raised towards the end of a public user registration request after a user is assigned to a default user group.

#### Params:

*   _user_ – A [UserModel](/api/v2/models/UserModel.html) object representing the user that was just registered.
*   _defaultGroupId_ \- The id of the user group that the user was just assigned to.