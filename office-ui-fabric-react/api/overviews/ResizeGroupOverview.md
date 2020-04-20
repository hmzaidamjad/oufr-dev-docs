---
title: Fabric ResizeGroup Overview | Microsoft Docs
author: Vitalius1
ms.author: vibraga
---

## Overview
ResizeGroup is a React component that can be used to help fit the right amount of content within a container. The consumer of the ResizeGroup provides some initial data, a reduce function, and a render function. The render function is responsible for populating the contents of a the container when given some data. The initial data should represent the data that should be rendered when the ResizeGroup has infinite width. If the contents returned by the render function do not fit within the ResizeGroup, the reduce function is called to get a version of the data whose rendered width should be smaller than the data that was just rendered.

An example scenario is shown below, where controls that do not fit on screen are rendered in an overflow menu. The data in this situation is a list of &#39;primary&#39; controls that are rendered on the top level and a set of overflow controls rendered in the overflow menu. The initial data in this case has all the controls in the primary set. The implementation of onReduceData moves a control from the overflow well into the primary control set.

This component queries the DOM for the dimensions of elements. Too many of these dimension queries will negatively affect the performance of the component and could cause poor interactive performance on websites. One way to reduce the number of measurements performed by the component is to provide a cacheKey in the initial data and in the return value of onReduceData. Two data objects with the same cacheKey are assumed to have the same width, resulting in measurements being skipped for that data object. In the controls with an overflow example, the cacheKey is simply the concatenation of the keys of the controls that appear in the top level.

There is a boolean context property (isMeasured) added to let child components know if they are only being used for measurement purposes. When isMeasured is true, it will signify that the component is not the instance visible to the user. This will not be needed for most scenarios. It is intended to be used to to skip unwanted side effects of mounting a child component more than once. This includes cases where the component makes API requests, requests focus to one of its elements, expensive computations, and&#x2F;or renders markup unrelated to its size. Be careful not to use this property to change the components rendered output in a way that effects it size in any way.



## Do &#10003;
- Ensure the width of the parent of this component has a fixed width that does not depend on the dimensions of it&#39;s children. Failure to do so may result in ResizeGroup attempting to fill a width of 0px.
- Ensure that result of rendering the data returned by onReduceData is actually smaller than the previous data.
- Include a cacheKey in your data to improve performance. Two objects with the same cacheKey are assumed to have the same width, so the ResizeGroup will only store one measurement per resize group.
- Implement onGrowData for improved performance when the container for the resize group increases in size.


## Don't &#10008;
- Do any DOM measurements inside your onReduce function as this will degrade performance
- Provide too many different return values for onReduce, it will degrade performance