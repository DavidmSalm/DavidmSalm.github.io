---
product: Excel
order: 2.2
---

# CUBEVALUE function

This article describes the formula syntax and usage of the CUBEVALUE function in Microsoft Excel.

## Description

Returns an aggregated value from the cube.

## Syntax

CUBEVALUE(connection, [member_expression1], [member_expression2], …)

The CUBEVALUE function syntax has the following arguments:

- **Connection**    Required. A text string of the name of the connection to the cube.
- **Member_expression**    Optional. A text string of a multidimensional expression (MDX) that evaluates to a member or tuple within the cube. Alternatively, member_expression can be a set defined with the CUBESET function. Use member_expression as a slicer to define the portion of the cube for which the aggregated value is returned. If no measure is specified in member_expression, the default measure for that cube is used.

## Remarks

- When the CUBEVALUE function evaluates, it temporarily displays a "#GETTING_DATA…" message in the cell before all of the data is retrieved.
- If a cell reference is used for member_expression, and that cell reference contains a CUBE function, then member_expression uses the MDX expression for the item in the referenced cell, and not the value displayed in that referenced cell.
- If the connection name is not a valid workbook connection stored in the workbook, CUBEVALUE returns a #NAME? error value. If the Online Analytical Processing (OLAP) server is not running, not available, or returns an error message, CUBEVALUE returns a #NAME? error value.
- If at least one element within the tuple is invalid, CUBEVALUE returns a #VALUE! error value.
- CUBEVALUE returns a #N/A error value when:
    - The member_expression syntax is incorrect.
    - The member specified by member_expression doesn't exist in the cube.
    - The tuple is invalid because there is no intersection for the specified values. (This can occur with multiple elements from the same hierarchy.
    - The set contains at least one member with a different dimension than the other members.
    - CUBEVALUE may return a #N/A error value if you reference a session-based object, such as a calculated member or named set, in a PivotTable when sharing a connection, and that PivotTable is deleted or you convert the PivotTable to formulas. (On the Options tab, in the Tools group, click OLAP Tools, and then click Convert to Formulas.)

**Issue: Null values are converted to zero-length strings**

In Excel, if a cell has no data because you never changed it or you deleted the contents, the cell contains an empty value. In many database systems, an empty value is called a Null value. An empty or Null value literally means "No value." However, a formula can never return an empty string or Null value. A formula always returns one of three values: a number value; a text value, which may be a zero-length string, or an error value, such as #NUM! or #VALUE.

If a formula contains a CUBEVALUE function connected to an Online Analytical Processing (OLAP) database and a query to this database results in a Null value, Excel converts this Null value to a zero-length string, even if the formula would otherwise return a number value. This can lead to a situation where a range of cells contain a combination of numeric and zero-length string values, and this situation can affect the results of other formulas that reference that range of cells. For example, if A1 and A3 contain numbers, and A2 contains a formula with a CUBEVALUE function that returns a zero-length string, the following formula would return a #VALUE! error:

=A1+A2+A3

To prevent this, you can test for a zero-length string by using the ISTEXT function and by using the IF function to replace the zero-length with a 0 (zero) as the following example shows:

=IF(ISTEXT(A1),0,A1)+IF(ISTEXT(A2),0,A2)+IF(ISTEXT(A3),0,A3)

Alternatively, you can nest the CUBEVALUE function in an IF condition that returns a 0 value if the CUBEVALUE function evaluates to a zero-length string as the following example shows:

=IF (CUBEVALUE("Sales","[Measures].[Profit]","[Time].[2004]","[All Product].[Beverages]")="", 0, CUBEVALUE("Sales","[Measures].[Profit]","[Time].[2004]","[All Product].[Beverages]"))

Note that the SUM function does not require this test for a zero-length string because it automatically ignores zero-length strings when calculating its return value.

## Examples

=CUBEVALUE("Sales","[Measures].[Profit]","[Time].[2004]","[All Product].[Beverages]")

=CUBEVALUE($A$1,"[Measures].[Profit]",D$12,$A23)

=CUBEVALUE("Sales",$B$7,D$12,$A23)