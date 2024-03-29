---
description: >-
  The missing documentation.
---

# Analysis Services Tabular/Power BI Compatibility Levels

Ever since the convergence of Analysis Services (Tabular) and Power BI (Premium) has started, the Analysis Services API surface has evolved rapidly. New versions of the [TOM client libraries](https://www.nuget.org/packages/Microsoft.AnalysisServices.retail.amd64/) are being released frequently, and new Power BI (Premium) features introduce new TOM Compatibility Levels which must be configured at the database in order to enable certain features in the underlying engine.

However, official documentation of those feature sets and the corresponding compatibility levels and NuGet versions is very sparse.

Using the reflection approach described [in this article](./list-new-features-in-tom-library.md), this page provides ***"The Missing Documentation"*** - a chronological listing of TOM features by compatibility level. Major features are annotated with related articles and blog posts.

- [Article: Compatibility level for tabular models](https://docs.microsoft.com/analysis-services/tabular-models/compatibility-level-for-tabular-models-in-analysis-services)

### API Changes in 19.65.7 - 6-Jul-2023

- Release of [TMDL Preview-3](https://www.nuget.org/packages/Microsoft.AnalysisServices.Tabular.Tmdl.retail.amd64/19.65.7.2-TmdlPreview) [[NetCore](https://www.nuget.org/packages/Microsoft.AnalysisServices.Tabular.Tmdl.NetCore.retail.amd64/19.65.7.2-TmdlPreview)]

*No changes in public API*

### API Changes in 19.65.4 - 8-Jun-2023

*No changes in public API*

### API Changes in 19.64 - 4-May-2023

- Release of [TMDL Preview-2](https://www.nuget.org/packages/Microsoft.AnalysisServices.Tabular.Tmdl.retail.amd64/19.64.0-TmdlPreview) [[NetCore](https://www.nuget.org/packages/Microsoft.AnalysisServices.Tabular.Tmdl.NetCore.retail.amd64/19.64.0-TmdlPreview)]
- REMOVED: `CalculationGroup.DefaultExpression` property *(Preview)*
- REMOVED: `CalculationExpression` class *(Preview)*
- ADDED: `CalculationGroupSelectionMode` enum *(Preview)*
- ADDED: `CalculationGroupExpression` class *(Preview)*
- ADDED: `CalculationGroup.MultiSelectionExpression` property *(Preview)*
- ADDED: `CalculationGroup.NoSelectionExpression` property *(Preview)*

### API Changes in 19.61 - 6-Apr-2023

- Release of [TMDL Preview-1](https://www.nuget.org/packages/Microsoft.AnalysisServices.Tabular.Tmdl.retail.amd64/19.61.1.4-TmdlPreview) [[NetCore](https://www.nuget.org/packages/Microsoft.AnalysisServices.Tabular.Tmdl.NetCore.retail.amd64/19.61.1.4-TmdlPreview)]
- NEW MetadataObject types:
  - `Calendar` *(Preview)*
  - `TimeUnitColumnAssociation` *(Preview)*
  - `CalendarColumnReference` *(Preview)*
- ADDED: `TimeUnit` enum *(Preview)*

## 1604 (new in 19.60 - 16-Mar-2023)

- ADDED: [`ModeType.DirectLake`](https://learn.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.modetype) *(used with [`Partition.Mode`](https://learn.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.partition.mode))*
- ADDED: [`EntityPartitionSource.SchemaName`](https://learn.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.entitypartitionsource.schemaname)

### API Changes in 19.60

- ADDED: [`JsonScripter.ScriptExport(Database)`](https://learn.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.jsonscripter.scriptexport): *Create a script to export a database.*

## 1603 (new in 19.57 - 9-Feb-2023)

- [DataCoverageDefinition](https://learn.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.datacoveragedefinition) MetadataObject type: *A tabular DataCoverageDefinition object. The expression defined on this object gives hint about the data in a partition.*
- [Partition.DataCoverageDefinition](https://learn.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.partition.datacoveragedefinition): *A reference to an optional DataCoverageDefinition that provides the hint regarding the data that is covered by the partition.*

### API Changes in 19.55 - 11-Jan-2022

*No changes in public API*

### API Changes in 19.54 - 8-Dec-2022

*No changes in public API*

### API Changes in 19.52 - 10-Nov-2022

- Remarks added to [Model.ApplyRefreshPolicies](https://learn.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.model.applyrefreshpolicies), [Table.ApplyRefreshPolicy](https://learn.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.table.applyrefreshpolicy):
> - The execution flow of ApplyRefreshPolicy is the same as the flow that is triggered by a call to RequestRefresh, followed by a call to Model.SaveChanges.
> - The execution flow includes:
>   - Calculating the expected partitioning scheme based on the effective date.
>   - Comparing the expected partitioning scheme to the existing set of partitions and issue the needed changes to move to the expected scheme [dropping, creating, and merging partitions as needed].
>   - Refresh the newly created partitions as well as the existing partitions in the incremental window.
> - The only difference between the ApplyRefreshPolicy and the combination of RequestRefresh+SaveChanges is ApplyRefreshPolicy enables advanced options that are not available otherwise.
> - It is recommended to use ApplyRefreshPolicy when you want to use a single API call, especially with advanced options.
> - It is recommended to use the combination of RequestRefresh + SaveChanges when you want to combine the operation with additional authoring calls in the model before the call to SaveChanges.

### API Changes in 19.51 - 6-Oct-2022

*No changes in public API*

## 1601 (new in 19.49 - 19-Sep-2022)

Introduces **FormatStringDefinition** to measures. Previously, those only existed on [Calculation Items](https://learn.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.calculationitem.formatstringdefinition).

- [Measure.FormatStringDefinition](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.measure.formatstringdefinition): *A reference to a FormatStringDefinition object owned by this Measure.*

### API Changes in 19.48 - 15-Aug-2022

- [Model.ApplyAutomaticAggregations](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.model.applyautomaticaggregations): *Retrieves automatic aggregation recommendations from Analysis Services engine and applies changes to the model.*
- [JsonScripter.ScriptApplyAutomaticAggregations](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.jsonscripter.scriptapplyautomaticaggregations): *Scripts out a given Tabular database into an ApplyAutomaticAggregations command.*

## 1572 (new in 19.46 - 11-Jul-2022)

- [Table.ExcludeFromAutomaticAggregations](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.table.excludefromautomaticaggregations): *An indication whether the table is excluded from the automatic aggregations feature.*

## 1571 (new in 19.42 - 20-Jun-2022)

- [ObjectTranslation.Altered](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.objecttranslation.altered): *An indication if the translation of the property had been changed.*

## 1570 (new in 19.42 - 20-Jun-2022)

- [NamedExpression.ExpressionSource](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.namedexpression.expressionsource): *A reference to the NamedExpression where the parameter associated with the remote model.*
- [NamedExpression.RemoteParameterName](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.namedexpression.remoteparametername): *The parameter name defined in source model, applicable only for proxy model and empty for local model.*

### API Changes in 19.39 - 13-Apr-2022

- New value in [TraceEventClass Enum](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.traceeventclass)
  - `DAXEvaluationLog = 135` *Output of EvaluateAndLog function.*
- New property in [ConnectionInfo Class](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.connectioninfo)
  - `public AsAzureRedirection AsAzureRedirection { get; }`

## 1569 (new in 19.36 - 01-Mar-2022)

- [Model.MaxParallelismPerQuery](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.model.maxparallelismperquery): *Maximum degree of parallelism for query in formula engine*

## 1568 (new in 19.36 - 01-Mar-2022)

- [Model.MaxParallelismPerRefresh](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.model.maxparallelismperrefresh): *Determines the max possible number of parallel tasks in data refresh, within the resource constraints of the hosting service.*

## 1567 (new in 19.34 - 10-Feb-2022)

- ChangedProperties ([Column](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.column.changedproperties), [Hierarchy](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.hierarchy.changedproperties), [Level](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.level.changedproperties), [Measure](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.measure.changedproperties), [Relationship](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.relationship.changedproperties), [Table](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.table.changedproperties)): *Represents an indication of a change to one of the object's properties.*

## 1566 (new in 19.27 - 10-Nov-2021)

- [Model.DisableAutoExists](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.model.disableautoexists): *Disable auto exists behavior for SummarizeColumns.*

## 1565 (new in 19.26 - 08-Sep-2021)

**Hybrid Tables in PBI Premium**

- <https://powerbi.microsoft.com/blog/announcing-public-preview-of-hybrid-tables-in-power-bi-premium/>
- <https://docs.microsoft.com/power-bi/connect-data/incremental-refresh-overview>
- <https://docs.microsoft.com/power-bi/connect-data/incremental-refresh-xmla>

API Changes

- [RefreshPolicyMode](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.refreshpolicymode) (Enum)
  - RefreshPolicyMode.Import: *Creates import partitions during incremental refresh.*
  - **RefreshPolicyMode.Hybrid**: *Creates import and DirectQuery partitions during incremental refresh.*
  - [RefreshPolicy.Mode](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.refreshpolicy.mode)

## 1564 (new in 19.22 - 02-Jun-2021)

- [AutomaticAggregationOptions](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.automaticaggregationoptions)
  - [Model.AutomaticAggregationOptions](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.model.automaticaggregationoptions)

## 1563 (new in 19.20 - 07-Apr-2021)

- [PartitionSourceType.Inferred](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.partitionsourcetype): *The data in this partition is populated by executing a query generated by the system.*
- [InferredPartitionSource](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.inferredpartitionsource): *Represents a Partition that its data will be populated by executing a query generated by the system.*
- [ParquetPartitionSource](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.parquetpartitionsource): *Represents a Partition that its data will be populated by executing a query generated by the system.*

## 1562 (new in 19.16 - 15-Jan-2021)

**Auto Aggregations**

- <https://powerbi.microsoft.com/blog/announcing-public-preview-of-automatic-aggregations/>
- <https://aka.ms/AutomaticAggregations>
- <https://docs.microsoft.com/power-bi/enterprise/aggregations-auto-configure>

API Changes

- [Table.SystemManaged](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.table.systemmanaged): *A boolean value that indicates whether the table is managed by the system. The system takes ownership of creation and deletion of such tables.*

## 1561 (new in 19.14 - 08-Dec-202)

- [SecurityFilteringBehavior.None](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.securityfilteringbehavior): *No filtering will occur from either end of the relationship.*

## 1560 (new in 19.12 - 12-Oct-2020)

- [Model.DiscourageCompositeModels](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.model.discouragecompositemodels): *Determines whether to discourage composite models.*

## 1550 (new in 19.9 - 10-Aug-2020)

- SourceLineageTag (Column, Hierarchy, Level, Measure, NamedExpression, Table): *A tag that represents the lineage of the source for the object.*

## 1545 (new 19.6 - 14-Jul-2020)

- [NamedExpression.ParameterValuesColumn](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.namedexpression.parametervaluescolumn): *Client tools apply filters to this column using M parameter. The presence of this property indicates model owner allows Dax queries to override this parameter, and columns data type must match the type specified in the meta tag of the parameter.*

## 1540 (new 19.4 - 16-Jun-2020)

- LineageTag (Column, Hierarchy, Level, Measure, NamedExpression, Table): *A tag that represents the lineage of the object.*

## 1535 (new in 19.2 - 01-Jun-2020)

- [Model.MAttributes](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.model.mattributes): *The string that has M attributes.*
- [NamedExpression.MAttributes](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.namedexpression.mattributes)

## 1520 (max level in 18.4)

- [Model.SourceQueryCulture](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.model.sourcequeryculture): *The name of the Culture used for formatting during refresh through Mashup.*

## 1510

- [Model.DataSourceDefaultMaxConnections](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.model.datasourcedefaultmaxconnections): *DataSourceDefaultMaxConnections will be used for connections to a data source if MaxConnections is set to -1 on the data source object or if there is no corresponding data source object for the data source.*

## 1500

- [CalculationItem.Ordinal](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.calculationitem.ordinal): *The zero-based ordinal value associated with a Calculation Item.*

## 1480

**QueryGroups**

- [Model.QueryGroups](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.model.querygroups): *Gets the collection object of all querygroups in the current Model.*
- [NamedExpression.QueryGroup](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.namedexpression.querygroup): *QueryGroup associated with the expression.*
- [Partition.QueryGroup](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.partition.querygroup): *QueryGroup associated with the partition.*
- [Table.ExcludeFromModelRefresh](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.table.excludefrommodelrefresh): *A boolean value that indicates whether the table is excluded from model refresh. When this is true, a refresh operation on the model would not trigger a refresh on the partitions of the table if they were already processed.*

## 1475

- [Model.DataSourceVariablesOverrideBehavior](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.model.datasourcevariablesoverridebehavior): *Controls whether this model allows data source variables to be overriden.*

## 1470

**Calculation Groups**

- <https://www.sqlbi.com/articles/introducing-calculation-groups/>
- <https://www.sqlbi.com/calculation-groups/>
- <https://docs.microsoft.com/analysis-services/tabular-models/calculation-groups>

API Changes

- [CalculationGroup](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.calculationgroup), [CalculationItem](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.calculationitem)
  - [Table.CalculationGroup](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.table.calculationgroup)
  - [PartitionSourceType.CalculationGroup](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.partitionsourcetype): *The partition uses CalculationGroup as a source.*
- [Model.DiscourageImplicitMeasures](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.model.discourageimplicitmeasures): *Determines whether to discourage the implicit measures.*

## 1465

**"Enhanced Metadata Format"**

- <https://docs.microsoft.com/power-bi/connect-data/desktop-enhanced-dataset-metadata>
- <https://powerbi.microsoft.com/blog/power-bi-september-2020-feature-summary/#Enhanced_Dataset_Metadata>
- <https://powerbi.microsoft.com/blog/power-bi-desktop-march-2020-feature-summary/#_Enhanced_dataset_metadata>

API Changes

- [PowerBIDataSourceVersion.PowerBI_V3](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.powerbidatasourceversion): *Power BI V3 Data Sources support basic partition management operations.*
- [Model.ForceUniqueNames](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.model.forceuniquenames): *Determines whether measures can have the same names as any column in the model.*
- [LinquisticMetadata.ContentType](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.linguisticmetadata.contenttype): *Specifies the type of the linguistic metadata from the Content property. E.g. XML or JSON.*

## 1460

- [SummarizationType](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.summarizationtype) (Enum: GroupBy, Sum, Count, Min, Max): Specifies the Summarization type to be used by alternative sources' columns.
  - [AlternateOf.Summarization](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.alternateof.summarization)
- [AlternateOf](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.alternateof): *Represents a AlternativeSource object. It is a child of either a Table or a Column object.*
  - [Column.AlternateOf](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.column.alternateof): *Defines the AlternateOf reference source BaseTable or BaseColumn, and the Summarization.*
- [Table.AlternateSourcePrecedence](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.table.alternatesourceprecedence): *The ranking or precedence used to select the alternate source table in case more than one match is found.*

## 1455

**Dual Storage Mode**

- <https://docs.microsoft.com/power-bi/transform-model/desktop-storage-mode>

API Changes

- [ModeType.Dual](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.modetype): *Allows support for dual mode of Import as well as DirectQuery.*
- [Measure.DataCategory](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.measure.datacategory): *Specifies the type of data contained in the measure so that you can add custom behaviors based on measure type.*

## 1450

**Incremental Refresh Policy (Import)**

- <https://docs.microsoft.com/power-bi/connect-data/incremental-refresh-configure>
- <https://powerbi.microsoft.com/blog/incremental-refresh-is-generally-available/>
- <https://blog.crossjoin.co.uk/2020/04/13/keep-the-existing-data-in-your-power-bi-dataset-and-add-new-data-to-it-using-incremental-refresh/>
- <https://radacad.com/all-you-need-to-know-about-the-incremental-refresh-in-power-bi-load-changes-only>

API Changes

- [Table.RefreshPolicy](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.table.refreshpolicy)
  - [BasicRefreshPolicy](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.basicrefreshpolicy)
  - [RefreshGranularityType](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.refreshgranularitytype) (Enum: *Day, Month, Quarter, Year*)
  - [RefreshPolicyType](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.refreshpolicytype) (Enum: *Basic*)
  - [PartitionSourceType.PolicyRange](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.partitionsourcetype): *The partition uses an M expression to retrieve the data. The partition ranges are auto created based on RefreshPolicy.*
  - [PolicyRangePartitionSource](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.policyrangepartitionsource)
- [PowerBIDataSourceVersion](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.powerbidatasourceversion) (Enum): *DataSource format version in Power BI Service.*
  - [Model.DefaultPowerBIDataSourceVersion](https://docs.microsoft.com/dotnet/api/microsoft.analysisservices.tabular.model.defaultpowerbidatasourceversion): *Used by PBIX data source format conversion.*

## Earlier Versions

- 1100, 1103, 1200, 1400
