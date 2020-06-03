---
description: >-
  How-to: Auto-generate a list of new features released in Analysis Services Tabular
---

# List new features in TOM Library

This week, a new version (19.2.0.2) of the Tabular Object Model (TOM) was [released on NuGet](https://www.nuget.org/packages/Microsoft.AnalysisServices.retail.amd64/). In fact, this is a major version jump from the previous 18.7 release:

![NuGet Version History](../../.gitbook/assets/list-new-features-in-tom-library-201122.png)

Those updates have been more frequent recently, and they usually coincide with new features coming to Azure Analysis Services or Power BI (Premium), and allow open-source tools like [Tabular Editor](https://tabulareditor.com/) to support those features.

So, those releases are generally very happy occasions and allow to become more productive with the wider Power BI ecosystem. However, it has always bothered me that the AMO/TOM libraries come with very sparse documentation, and virtually no change log or release notes. Lots of the functionality has to be discovered the hard way by trial and error.

The 19.x version number has made me particularly curious this time, and I wanted to know more. I remembered from previous use of TOM that I would occasionally receive a `CompatibilityViolationException` when running some code against an older on-prem Tabular server, for instance.

Taking a quick look inside reveals that certain parts of the API are annotated with an `CompatibilityRequirement` attribute:

![CompatibilityRequirementAttribute](../../.gitbook/assets/list-new-features-in-tom-library-202423.png)

The number referenced there clearly corresponds to the well-known [Compatibility Level for tabular models](https://docs.microsoft.com/analysis-services/tabular-models/compatibility-level-for-tabular-models-in-analysis-services). *Remember, 1200 was the level that properly kicked off tabular modeling with the introduction of TMSL, and 1400 was another milestone with the [introduction of M/PowerQuery integration into Tabular and structured data sources](https://docs.microsoft.com/archive/blogs/analysisservices/supporting-advanced-data-access-scenarios-in-tabular-1400-models).*

Since the tight integration between Analysis Services and Power BI, quite a few new compatibility levels have been introduced, very recently 1520 in conjunction with the [enhanced PBIX metadata format](https://docs.microsoft.com/power-bi/desktop-enhanced-dataset-metadata).

So it didn't take very long to write a little LinqPad script to automatically extract and list all TOM API elements that happen to be annotated with that `[CompatibilityRequirement]` attribute.

Turns out the attribute internally distinguishes three different scopes, namely: "Box" (presumably on-prem SSAS), "Excel", and "Pbi". Since 1400 has been around for a long time now, the results are split into two tables below: All features introduced after 1400 first, then all others that are marked with _1200_, _1400_, or _Unsupported_.

How to read this? To take an example, the features around [Calculation Groups](https://www.sqlbi.com/articles/introducing-calculation-groups/) were introduced with level `1470` (which is also mentioned in [Kaspar's blog post here](https://www.kasperonbi.com/adding-calculation-groups-to-aas-or-pbi-premium/)). However, the ability to define a custom sort order for calculation items via the `Ordinal` property was only introduced with the `1500` level, hence requires a model at that level and a server that supports it as well.

Having checked one of our production Azure Analysis Services servers, they currently report those _SupportedCompatibilityLevels_: `1100,1103,1200,1400,1450,1455,1460,1465,1470,1475,1480,1500,1510,1520,1530,1000000`, i.e. go up as far as **1530**.

Hence, it is very interesting to see one item listing "1535" below (`MAttributes`), and even some explicitly marked as "Preview" (`AnalyticsAIMetadata`).

What is going to be announced here??

In any case, I hope this helps some folks (like myself) getting a bit more clarity about the features supported by the TOM API, and how those relate to the various new compatibility levels that were introduced post-1400.

The script used here is provided at the bottom, and can easily be re-run with any future releases appearing on NuGet!

## Table 1: Post-1400 Features

| Name | MemberType | Box | Excel | Power BI |
| :--- | :--- | --- | --- | --- |
| Microsoft.AnalysisServices.Tabular.AlternateOf | Class | 1460 | 1460 | 1460 |
| Microsoft.AnalysisServices.Tabular.AlternateOfAnnotationCollection | Class | 1460 | 1460 | 1460 |
| Microsoft.AnalysisServices.Tabular.AnalyticsAIMetadata | Class | Preview | Preview | Preview |
| Microsoft.AnalysisServices.Tabular.AnalyticsAIMetadataCollection | Class | Preview | Preview | Preview |
| Microsoft.AnalysisServices.Tabular.BasicRefreshPolicy | Class | 1450 | 1450 | 1450 |
| Microsoft.AnalysisServices.Tabular.CalculationGroup | Class | 1470 | 1470 | 1470 |
| Microsoft.AnalysisServices.Tabular.CalculationGroupAnnotationCollection | Class | 1470 | 1470 | 1470 |
| Microsoft.AnalysisServices.Tabular.CalculationGroupSource | Class | 1470 | 1470 | 1470 |
| Microsoft.AnalysisServices.Tabular.CalculationItem | Class | 1470 | 1470 | 1470 |
| Microsoft.AnalysisServices.Tabular.CalculationItem.Ordinal | Property | 1500 | 1500 | 1500 |
| Microsoft.AnalysisServices.Tabular.CalculationItemCollection | Class | 1470 | 1470 | 1470 |
| Microsoft.AnalysisServices.Tabular.Column.AlternateOf | Property | 1460 | 1460 | 1460 |
| Microsoft.AnalysisServices.Tabular.ContentType | Enum | 1465 | 1465 | 1465 |
| Microsoft.AnalysisServices.Tabular.DataSourceVariablesOverrideBehaviorType | Enum | 1475 | 1475 | 1475 |
| Microsoft.AnalysisServices.Tabular.FormatStringDefinition | Class | 1470 | 1470 | 1470 |
| Microsoft.AnalysisServices.Tabular.LinguisticMetadata.ContentType | Property | 1465 | 1465 | 1465 |
| Microsoft.AnalysisServices.Tabular.Measure.DataCategory | Property | 1455 | 1455 | 1455 |
| Microsoft.AnalysisServices.Tabular.Model.AnalyticsAIMetadata | Property | Preview | Preview | Preview |
| Microsoft.AnalysisServices.Tabular.Model.DataSourceDefaultMaxConnections | Property | 1510 | 1510 | 1510 |
| Microsoft.AnalysisServices.Tabular.Model.DataSourceVariablesOverrideBehavior | Property | 1475 | 1475 | 1475 |
| Microsoft.AnalysisServices.Tabular.Model.DefaultPowerBIDataSourceVersion | Property | 1450 | 1450 | 1450 |
| Microsoft.AnalysisServices.Tabular.Model.DiscourageImplicitMeasures | Property | 1470 | 1470 | 1470 |
| Microsoft.AnalysisServices.Tabular.Model.ForceUniqueNames | Property | 1465 | 1465 | 1465 |
| Microsoft.AnalysisServices.Tabular.Model.MAttributes | Property | 1535 | 1535 | 1535 |
| Microsoft.AnalysisServices.Tabular.Model.QueryGroups | Property | 1480 | 1480 | 1480 |
| Microsoft.AnalysisServices.Tabular.Model.SourceQueryCulture | Property | 1520 | 1520 | 1520 |
| Microsoft.AnalysisServices.Tabular.ModeType.Dual | Field | 1455 | 1455 | 1455 |
| Microsoft.AnalysisServices.Tabular.NamedExpression.MAttributes | Property | 1535 | 1535 | 1535 |
| Microsoft.AnalysisServices.Tabular.NamedExpression.ParameterValuesColumn | Property | Preview | Preview | Preview |
| Microsoft.AnalysisServices.Tabular.NamedExpression.QueryGroup | Property | 1480 | 1480 | 1480 |
| Microsoft.AnalysisServices.Tabular.Partition.QueryGroup | Property | 1480 | 1480 | 1480 |
| Microsoft.AnalysisServices.Tabular.PartitionSourceType.CalculationGroup | Field | 1470 | 1470 | 1470 |
| Microsoft.AnalysisServices.Tabular.PartitionSourceType.PolicyRange | Field | 1450 | 1450 | 1450 |
| Microsoft.AnalysisServices.Tabular.PolicyRangePartitionSource | Class | 1450 | 1450 | 1450 |
| Microsoft.AnalysisServices.Tabular.PowerBIDataSourceVersion | Enum | 1450 | 1450 | 1450 |
| Microsoft.AnalysisServices.Tabular.PowerBIDataSourceVersion.PowerBI_V3 | Field | 1465 | 1465 | 1465 |
| Microsoft.AnalysisServices.Tabular.QueryGroup | Class | 1480 | 1480 | 1480 |
| Microsoft.AnalysisServices.Tabular.QueryGroupAnnotationCollection | Class | 1480 | 1480 | 1480 |
| Microsoft.AnalysisServices.Tabular.QueryGroupCollection | Class | 1480 | 1480 | 1480 |
| Microsoft.AnalysisServices.Tabular.RefreshGranularityType | Enum | 1450 | 1450 | 1450 |
| Microsoft.AnalysisServices.Tabular.RefreshPolicy | Class | 1450 | 1450 | 1450 |
| Microsoft.AnalysisServices.Tabular.RefreshPolicyAnnotationCollection | Class | 1450 | 1450 | 1450 |
| Microsoft.AnalysisServices.Tabular.RefreshPolicyExtendedPropertyCollection | Class | 1450 | 1450 | 1450 |
| Microsoft.AnalysisServices.Tabular.RefreshPolicyType | Enum | 1450 | 1450 | 1450 |
| Microsoft.AnalysisServices.Tabular.SummarizationType | Enum | 1460 | 1460 | 1460 |
| Microsoft.AnalysisServices.Tabular.Table.AlternateSourcePrecedence | Property | 1460 | 1460 | 1460 |
| Microsoft.AnalysisServices.Tabular.Table.CalculationGroup | Property | 1470 | 1470 | 1470 |
| Microsoft.AnalysisServices.Tabular.Table.ExcludeFromModelRefresh | Property | 1480 | 1480 | 1480 |
| Microsoft.AnalysisServices.Tabular.Table.RefreshPolicy | Property | 1450 | 1450 | 1450 |

## Table 2: 1200/1400 Features

| Name | MemberType | Box | Excel | Power BI |
| :--- | :--- | --- | --- | --- |
| Microsoft.AnalysisServices.Tabular.AttributeHierarchy.ExtendedProperties | Property | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.AttributeHierarchyExtendedPropertyCollection | Class | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.CalculatedPartitionSource.RetainDataTillForceCalculate | Property | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.Column.EncodingHint | Property | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.Column.ExtendedProperties | Property | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.Column.RelatedColumnDetails | Property | Unsupported | Unsupported | 1400 |
| Microsoft.AnalysisServices.Tabular.Column.Variations | Property | 1400 | 1400 | 1200 |
| Microsoft.AnalysisServices.Tabular.ColumnExtendedPropertyCollection | Class | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.ColumnPermission | Class | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.ColumnPermissionAnnotationCollection | Class | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.ColumnPermissionCollection | Class | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.ColumnPermissionExtendedPropertyCollection | Class | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.ConnectionAddress | Class | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.ConnectionDetails | Class | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.Credential | Class | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.Culture.ExtendedProperties | Property | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.CultureExtendedPropertyCollection | Class | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.DataAccessOptions | Class | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.DataRefresh.MPartitionSourceOverride | Class | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.DataRefresh.NamedExpressionOverride | Class | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.DataRefresh.StructuredDataSourceOverride | Class | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.DataSource.ExtendedProperties | Property | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.DataSourceExtendedPropertyCollection | Class | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.DataSourceOptions | Class | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.DataSourceType.Structured | Field | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.DetailRowsDefinition | Class | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.EncodingHintType | Enum | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.EntityPartitionSource | Class | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.ExpressionKind | Enum | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.ExtendedProperty | Class | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.ExtendedPropertyType | Enum | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.GroupByColumn | Class | Unsupported | Unsupported | 1400 |
| Microsoft.AnalysisServices.Tabular.GroupByColumnCollection | Class | Unsupported | Unsupported | 1400 |
| Microsoft.AnalysisServices.Tabular.Hierarchy.ExtendedProperties | Property | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.Hierarchy.HideMembers | Property | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.HierarchyExtendedPropertyCollection | Class | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.HierarchyHideMembersType | Enum | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.JsonExtendedProperty | Class | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.KPI.ExtendedProperties | Property | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.KPIExtendedPropertyCollection | Class | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.Level.ExtendedProperties | Property | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.LevelExtendedPropertyCollection | Class | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.LinguisticMetadata.ExtendedProperties | Property | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.LinguisticMetadataExtendedPropertyCollection | Class | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.Measure.DetailRowsDefinition | Property | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.Measure.ExtendedProperties | Property | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.MeasureExtendedPropertyCollection | Class | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.MetadataPermission | Enum | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.Model.DefaultMeasure | Property | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.Model.Expressions | Property | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.Model.ExtendedProperties | Property | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.ModelExtendedPropertyCollection | Class | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.ModelRole.ExtendedProperties | Property | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.ModelRoleExtendedPropertyCollection | Class | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.ModelRoleMember.ExtendedProperties | Property | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.ModelRoleMemberExtendedPropertyCollection | Class | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.ModeType.Push | Field | Unsupported | Unsupported | 1200 |
| Microsoft.AnalysisServices.Tabular.MPartitionSource | Class | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.NamedExpression | Class | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.NamedExpressionAnnotationCollection | Class | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.NamedExpressionCollection | Class | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.NamedExpressionExtendedPropertyCollection | Class | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.ObjectState.ForceCalculationNeeded | Field | Unsupported | Unsupported | 1400 |
| Microsoft.AnalysisServices.Tabular.Partition.ExtendedProperties | Property | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.Partition.RetainDataTillForceCalculate | Property | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.PartitionExtendedPropertyCollection | Class | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.PartitionSourceType.Entity | Field | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.PartitionSourceType.M | Field | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.Perspective.ExtendedProperties | Property | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.PerspectiveColumn.ExtendedProperties | Property | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.PerspectiveColumnExtendedPropertyCollection | Class | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.PerspectiveExtendedPropertyCollection | Class | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.PerspectiveHierarchy.ExtendedProperties | Property | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.PerspectiveHierarchyExtendedPropertyCollection | Class | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.PerspectiveMeasure.ExtendedProperties | Property | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.PerspectiveMeasureExtendedPropertyCollection | Class | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.PerspectiveSet | Class | Unsupported | Unsupported | 1400 |
| Microsoft.AnalysisServices.Tabular.PerspectiveSetAnnotationCollection | Class | Unsupported | Unsupported | 1400 |
| Microsoft.AnalysisServices.Tabular.PerspectiveSetCollection | Class | Unsupported | Unsupported | 1400 |
| Microsoft.AnalysisServices.Tabular.PerspectiveSetExtendedPropertyCollection | Class | Unsupported | Unsupported | 1400 |
| Microsoft.AnalysisServices.Tabular.PerspectiveTable.ExtendedProperties | Property | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.PerspectiveTable.PerspectiveSets | Property | Unsupported | Unsupported | 1400 |
| Microsoft.AnalysisServices.Tabular.PerspectiveTableExtendedPropertyCollection | Class | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.RelatedColumnDetails | Class | Unsupported | Unsupported | 1400 |
| Microsoft.AnalysisServices.Tabular.Relationship.ExtendedProperties | Property | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.RelationshipExtendedPropertyCollection | Class | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.Set | Class | Unsupported | Unsupported | 1400 |
| Microsoft.AnalysisServices.Tabular.SetAnnotationCollection | Class | Unsupported | Unsupported | 1400 |
| Microsoft.AnalysisServices.Tabular.SetCollection | Class | Unsupported | Unsupported | 1400 |
| Microsoft.AnalysisServices.Tabular.SetExtendedPropertyCollection | Class | Unsupported | Unsupported | 1400 |
| Microsoft.AnalysisServices.Tabular.StringExtendedProperty | Class | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.StructuredDataSource | Class | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.Table.DefaultDetailRowsDefinition | Property | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.Table.ExtendedProperties | Property | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.Table.IsPrivate | Property | 1400 | 1400 | 1200 |
| Microsoft.AnalysisServices.Tabular.Table.Sets | Property | Unsupported | Unsupported | 1400 |
| Microsoft.AnalysisServices.Tabular.Table.ShowAsVariationsOnly | Property | 1400 | 1400 | 1200 |
| Microsoft.AnalysisServices.Tabular.TableExtendedPropertyCollection | Class | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.TablePermission.ColumnPermissions | Property | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.TablePermission.ExtendedProperties | Property | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.TablePermission.MetadataPermission | Property | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.TablePermissionExtendedPropertyCollection | Class | 1400 | 1400 | 1400 |
| Microsoft.AnalysisServices.Tabular.Variation | Class | 1400 | 1400 | 1200 |
| Microsoft.AnalysisServices.Tabular.VariationAnnotationCollection | Class | 1400 | 1400 | 1200 |
| Microsoft.AnalysisServices.Tabular.VariationCollection | Class | 1400 | 1400 | 1200 |
| Microsoft.AnalysisServices.Tabular.VariationExtendedPropertyCollection | Class | 1400 | 1400 | 1400 |

## LINQPad script

_Requires NuGet package: `Microsoft.AnalysisServices.retail.amd64` and namespace declaration: `TOM = Microsoft.AnalysisServices.Tabular`._

* `TOM-Extract-CompatibilityRequirementsAttributes.linq` file available in [this Gist](https://gist.github.com/mthierba/4147d1cb94e67086a201dce472e3daf5).

```csharp
var asm = typeof(TOM.Server).Assembly;
var compatAttr = asm.GetType("Microsoft.AnalysisServices.Tabular.CompatibilityRequirementAttribute");

string ReadProperty(string name, object attr) => compatAttr.GetProperty(name).GetValue(attr).ToString();
string GetMemberType(Type t) => t.IsEnum ? "Enum" : t.IsInterface ? "Interface" : "Class";
Attribute GetCustomAttributeSafe(MemberInfo member, Type t) 
{ // This is needed to avoid errors on a few specific attributes containing unsupported expressions - we're simply ignoring those
    try {
        return member.GetCustomAttribute(t);
    }
    catch (TOM.TomException) {
        return null;
    }
}

var members = asm.GetTypes()
    .Select(t => new
    {
        Type = t,
        CompatAttribute = t.GetCustomAttribute(compatAttr)
    })
    .Where(x => x.CompatAttribute != null)
    .Select(x => new 
    {
        Name = x.Type.FullName,
        MemberType = GetMemberType(x.Type),
        Box = ReadProperty("Box", x.CompatAttribute),
        Excel = ReadProperty("Excel", x.CompatAttribute),
        PBI = ReadProperty("Pbi", x.CompatAttribute)
    })
    .Union(
        asm.GetTypes()
        .SelectMany(t => t.GetMembers())
        .Select(m => new 
        {
            Member = m,
            Name = $"{m.DeclaringType.FullName}.{m.Name}",
            CompatAttribute = GetCustomAttributeSafe(m, compatAttr),
            MemberType = m.MemberType.ToString()
        })
        .Where(x => x.CompatAttribute != null)
        .Select(x => new
        {
            x.Name, 
            x.MemberType,
            Box = ReadProperty("Box", x.CompatAttribute),
            Excel = ReadProperty("Excel", x.CompatAttribute),
            PBI = ReadProperty("Pbi", x.CompatAttribute)
        })
    )
    .Where(x => /* toggle this for 1200/1400: */ !(((int.TryParse(x.Box, out var box) && box <= 1400) || x.Box == "Unsupported")
        && ((int.TryParse(x.Excel, out var excel) && excel <= 1400) || x.Excel == "Unsupported")
        && ((int.TryParse(x.PBI, out var pbi) && pbi <= 1400) || x.PBI == "Unsupported")))
    .OrderBy(x => x.Name)
    .ToArray()
    .Dump();

// Convert to markdown table for blog post:
Array.ForEach(members, x => $"| {x.Name} | {x.MemberType} | {x.Box} | {x.Excel} | {x.PBI} |".Dump());
```