# ragged-array-idioms

Repo to document the ragged array idioms in oceanography.

## Motivation

The purpose of this repository is to document the typical use patterns
for ragged array (i.e. Lagrangian) data in oceanography, as well as
the desired [Xarray](https://xarray.dev) syntax.
These idioms, together with those from other disciplines such as
high-energy physics, genomics, and others, will allow the ragged array
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

### Getting a variable

To access the entire ragged array of any given variable (in this example, `lon`),
use the usual `Dataset` field accessors:

```python
ds.lon
```

or

```python
ds['lon']
```

The result is a `DataArray` instance that is ragged internally.

This currently works with the Xarray Dataset generated by the CloudDrift
library, with the caveat that internally each variable (e.g. `lon`, `lat`) is
a long 1-d array of concatenated variable-length arrays.

### Indexing and selecting data

#### Selecting by dimension position and integer index

A variable, for example `lon`,  of the first trajectory can be selected by
integer index:

```python
ds.lon[0]
```

which is equivalent to:

```python
ds.lon[0,:]
```

Because `ds.lon` is internally ragged, each `ds.lon[0,:]`, `ds.lon[1,:]`, etc.,
may return a 1-d array of different length.

This currently does not work.

#### Selecting by dimension name and integer index

We can also select by dimension name, as with regular DataArrays.
The common dimensions of a typical oceanographic Lagrangian dataset are
`trajectory` and `obs` for observations, so we can use these dimensions accordingly:

```
ds.lon.isel(trajectory=0) # equivalent to ds.lon[0]
```

The result of this is the same as the above example of `ds.lon[0]`:
a 1-d array with varying length depending on the trajectory index.

The use case of this syntax for the trajectory dimension may be less effective
than that with normal multi-dimensional Xarray Datasets because there may not
be a meaningful "trajectory space" other than integer indexes.
So, this syntax may simply be a redundant form of the positional dimension
lookup syntax.

However, for the observation dimension, it would be quite useful. For example:

```
ds.lon.isel(obs=0)
```

would return the first element of the `lon` field for each trajectory.

This currently does not work.

#### Selecting by dimension name and value

Assuming that the `time` coordinate is defined as an array of `datetime`
instances, we can select by time value, e.g.:

```python
ds.lon.sel(time=datetime(2012, 9, 1))
```

This would return the variable as a DataArray for all trajectories on 00 UTC
September 1, 2012.

An outstanding question is whether the resultant DataArray contains only data
from those trajectories that had a valid value at this time, or does is it
filled with NaNs for trajectories that did not have a valid value at the
requested time.
This exception does not occur with normal Xarray DataArrays because they are
structured and multi-dimensional, i.e. the data is defined in other dimensions
for all values of time that are in range.

This currently does not work.

#### Selecting by label

If the `trajectory` coordinate is a labeled one (a set of strings instead of
integers), then selecting by labels works as with usual DataArrays:

```python
ds.lon.sel(trajectory='CARTHE123')
```

returns a DataArray of longitude for the `CARTHE123` trajectory.

This currently does not work.

### Geographical binning of a variable

This and examples below will be based on Philippe's EarthCube notebook snippets,
except that here we want to write down the desired syntax that currently doesn't
work, rather than the syntax with workarounds that works.


TODO

### Extract a region

TODO

### Operations per trajectory

TODO
