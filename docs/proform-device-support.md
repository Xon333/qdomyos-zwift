# ProForm device integration

Reference extracted from the existing repository instructions; use only when
adding a ProForm treadmill model. Follow the closest implementation in
`src/devices/proformtreadmill/` and retain original packet evidence.

The documented ProForm 995i capture uses an **11-byte BLE header**. Strip exactly
that capture header, preserve every payload and its order, and identify the
actual packet where initialization ends and `sendPoll` begins. Do not assume
all unrelated capture formats have this header. The recorded 995i example has
25 initialization frames (pkt4658–pkt4756), 33 sendPoll frames
(pkt4761–pkt4897), and a six-case poll cycle; a different capture defines its own
counts and boundary.

A new model requires the corresponding flag/default in the device header and
QZSettings, ordered QML property/catalog updates, and the existing model picker
selection/reset behavior. Load the flag in the constructor. Put initialization
frames in `btinit()` and recurring frames in the `sendPoll()` cycle. Include the
model in applicable `forceIncline()`/`forceSpeed()` conditions; reuse the exact
protocol family rather than guessing its checksum or command layout.

Verify frame bytes/order, the init/poll boundary, model selection and settings
consistency using the capture and relevant device fixtures. The source/capture
establishes device behavior; these historical frame counts are not a substitute
for a target-device check.
