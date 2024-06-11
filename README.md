# B2NDIO - simple multi-dimensional seismic data format

## The B2NDIO Seismic Format
The format uses the [CBlosc2 NDIM API](https://github.com/Blosc/c-blosc2) with an extra metadata layer named
"b2ndseis" that contains:

- For 3D volumes:
    - B2NDSEISFILE_METALAYER_VERSION (uint8_t)
    - 1 for 3D (uint8_t)
    - Z domain 0 or 1, 0=time, 1=depth (uint8_t)
    - inline,crossline start, stop, step (array 6 x int64_t)
    - Z start, step, nrsamples (array 2 x float + 1 x int32_t)
    - zunit (string (<=2^5-1 chars))
    - Data is stored as Blosc2 NDIM array with shape:
	    - NrComponents(<=2^4-1) x NrInlines x NrCrosslines x NrZsamples

- For 2D lines:
    - B2NDSEISFILE_METALAYER_VERSION (uint8_t)
    - 0 for 2D (uint_8)
    - Z domain 0 or 1, 0=time, 1=depth (uint8_t)
    - trace number start, stop, step (array 3 x int64_t)
    - Z start, step, nrsamples (array 2 x float + 1 x int32_t)
    - zunit (string (<=2^5-1 chars))
    - Data is stored as Blosc2 NDIM array with shape:
	    - NrComponents(<=2^4-1) x NrTraces x NrZsamples

Both file types have a vlmetadata layer named "b2ndseisvl" that contains:

- component names (array NrComponents(<=2^4-1) x string(<=2^8-1 chars))

Information is encoded in MsgPack format.
