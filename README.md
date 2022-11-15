# ragged-array-idioms

Repo to document the ragged array idioms in oceanography.

## Motivation

The purpose of this repository is to document the typical use patterns
for ragged array (i.e. Lagrangian) data in oceanography, as well as
the desired [Xarray](https://xarray.dev) syntax.
These idioms, together with those from other disciplines such as
high-energy physics, genomis, and others, will allow the ragged array
scientific community to evaluate the feasability of providing such
syntax in Xarray.

## Assumptions

* An Xarray Dataset `ds` contains Lagrangian (ragged array) data.
* The data consists of N variable-length 1-dimensional arrays
for each field (e.g. longitude, latitude, temperature, etc.)
* For ocean data purposes, call each variable-length array a _trajectory_.
* It's not important how the data is stored internally
(e.g. as a 1-d array, Awkward Array, or similar);
here we only want to document the idioms and the desired end-user syntax.

## Idioms

### Get a field

To access the entire ragged array of any given field, use the usual
Xarray Dataset field accessors:

```python
ds.lon
```

or

```python
ds['lon']
```

The result is an `xarray.DataArray` instance that is ragged internally.

### Get a field of a trajectory

### Get a field of a slice of trajectories

### Get all fields for a trajectory
