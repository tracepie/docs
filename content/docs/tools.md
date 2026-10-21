+++
title = "Choosing a tool"
description = "The intended roles of Tracepie's seven operations."
weight = 20
+++

The planned CLI and MCP interfaces share seven operations. This table describes their intended roles; it is not a released API contract.

| Operation | Use it to |
| --- | --- |
| `search` | Discover where a feature, name, symptom or requirement lives. |
| `inspect` | Examine a known entity, file, admitted source or result member. |
| `map` | Enumerate a scope or calculate structural properties. |
| `trace` | Follow directed relationships from a known starting point. |
| `impact` | Identify review obligations caused by a specified change. |
| `compare` | Describe differences or shared structure between selected subjects or revisions. |
| `investigate` | Gather evidence for a question that requires several explicit checks. |

## A typical investigation

Begin with `search` when the starting point is unknown. Use `inspect` to understand a returned subject. Choose `map` for an inventory or structural question, and `trace` for a path question.

Use `impact` when there is a concrete change to review, `compare` for an explicit comparison, and `investigate` when a question needs several checks.

## Interpret the result

Read the evidence basis, coverage and freshness with the answer. An incomplete result or missing provider cannot establish that a relationship does not exist. The agent remains responsible for interpreting the evidence and editing source files.
