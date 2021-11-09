# GrapheneOS App Incompatibility List

In the past, this location was used to try to create a short list of apps that had been found to run or not to run on GrapheneOS and to what degree.

The game has changed when GrapheneOS implemented compatibility shims to coerce Play Services into running as a regular app with no special access or privileges. This allows Play Services and its associated libraries to be optionally added to profiles on a case-by-case basis and run as a sandboxed, isolated, ordinary app without any privacy or security concerns at all. They can be added, deleted, frozen when not in use, or simply skipped altogether. Play Services, when sandboxed, behaves as a regular app and cannot do anything that any other app, when given the appropriate permissions, could not simply do itself.

Essentially, GrapheneOS allows you to have your cake and eat it too, and leaves the power to make this choice in your hands. This was deemed to be a safer approach than MicroG, which only emulates a small subset of Play Services, is still reliant on the same closed-source libraries, and requires deep and systematic integration with the operating system as a privileged system app in order to do its work.

As a result, it is no longer feasible for us to enumerate which apps work, and instead, for us to start looking into what apps *don't*.

At this moment, Play Services function for the majority of use cases, with some known limitations due to shims that have not yet been added, but are under active and continual development.

