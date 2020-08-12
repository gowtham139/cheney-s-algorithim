# cheney-s-algorithim
algorithm for implementing cheney's algorithm

initialize() =
    tospace   = N/2
    fromspace = 0
    allocPtr  = fromspace
    scanPtr   = whatever -- only used during collection

allocate(n) =
    If allocPtr + n > fromspace+ N/2
        collect()
    EndIf
    If allocPtr + n > fromspace+ N/2
        fail “insufficient memory”
    EndIf
    o = allocPtr
    allocPtr = allocPtr + n
    return o

collect() =
    swap(fromspace, tospace)
    allocPtr = fromspace
    scanPtr  = fromspace

    -- scan every root you've got
    ForEach root in the stack -- or elsewhere
        root = copy(root)
    EndForEach
    
    -- scan objects in the heap (including objects added by this loop)
    While scanPtr < allocPtr
        ForEach reference r from o (pointed to by scanPtr)
            r = copy(r)
        EndForEach
        scanPtr = scanPtr  + o.size() -- points to the next object in the heap, if any
    EndWhile

copy(o) =
    If o has no forwarding address
        o' = allocPtr
        allocPtr = allocPtr + size(o)
        copy the contents of o to o'
        forwarding-address(o) = o'
    EndIf
    return forwarding-address(o)

