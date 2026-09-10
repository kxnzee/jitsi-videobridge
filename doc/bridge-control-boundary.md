# Bridge control boundary (bridge side)

This document describes the `jitsi-videobridge` (JVB) side of the boundary
between bridge selection and the `jitsi-videobridge` instance that ends up
hosting a conference: which code receives and processes the colibri2 control
request sent by `jitsi-control` (Jicofo) once it has selected this bridge. For
the wire format of that control request, see the existing
[`rest-colibri2.md`](./rest-colibri2.md) reference — this document does not
repeat that content.

## Receiving the control request

`Conference`'s constructor builds a `Colibri2ConferenceHandler`
(`jvb/src/main/java/org/jitsi/videobridge/Conference.java:324`) and a
`ColibriQueue`
(`jvb/src/main/java/org/jitsi/videobridge/Conference.java:325`). The queue's
request handler receives each incoming colibri2 request (logged as `"RECV
colibri2 request: ..."`,
`jvb/src/main/java/org/jitsi/videobridge/Conference.java:346`) and dispatches
it to `Colibri2ConferenceHandler.handleConferenceModifyIQ`
(`jvb/src/main/java/org/jitsi/videobridge/Conference.java:349`), which creates
or updates the conference on this bridge.

## Selecting side

The bridge that receives this request was chosen by `jitsi-control`'s
`BridgeSelector.selectBridge`, which then sends the request via a
`Colibri2Session`. See
[`jicofo/doc/bridge-control-boundary.md`](https://github.com/kxnzee/jicofo/blob/master/doc/bridge-control-boundary.md)
for the full selecting-side detail, owned by that repository.

## See also

- [`jicofo/doc/bridge-control-boundary.md`](https://github.com/kxnzee/jicofo/blob/master/doc/bridge-control-boundary.md) —
  the selecting-side detail in the `jicofo` repository.
- [`rest-colibri2.md`](./rest-colibri2.md) — the colibri2 protocol reference;
  unrelated to which internal class dispatches the request, listed for
  orientation only.
