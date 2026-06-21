# ZVM_IO



##New I/O Instructions

The architecture now supports standard system bus hardware requests to handle downstream peripheral communication:

- `IN(VMP, port, function, argc)`: Direct Hardware Input. Signals a specified virtual hardware `port` to execute a designated `function`. Once the external device processes the request, the resulting input data bytes (`argc`) are safely harvested from the device's output buffers and pushed directly onto the ZVM stack memory layout.

---

# Integrated Safeguards

To prevent hardware bus locks and memory corruption, severe boundary checks have been embedded into the I/O execution pipeline:

- **Port Boundary Check (`BAD_MEMORY_ADDRESS`)**: Checks if the target device port address exceeds the maximum allowed device matrix constraint (`ZVM_IO_MAX_DEVICES`).
- **Device Availability Check (`IO_DEVICE_NOT_FOUND`)**: Halts operation if the requested hardware port points to an unmapped or disconnected device module.
- **Hardware Type Enforcement**: Asserts that the device mapped to the designated port explicitly supports input streams (`ZVM_IO_DEVICE_TYPE_IN` or `ZVM_IO_DEVICE_TYPE_INOUT`).

