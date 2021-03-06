# Hangzhou Kincony Electronics Co., Ltd. KC868-COL series controller hex protocol documentation v1.1

<strong>Be careful interacting with [Hangzhou Kincony](https://kincony.com). They have has a very strange way of interacting with its potential customers. I submitted substantially improved documentation (as in, understandable English, removing cache files, ...) as github issues. I repeatedly politely pointed them at it, using email and forum posts. First, they chose to ignore it, then to delete my posts from their forum. In short, you might get decent hardware, but don't expect anything resembling customer service.</strong>

Based upon a google translation of the Chinese language documentation. The original documentation was provided to me by the [https://kincony.com/forum admin](xuwei@kincony.com).

Note that the KC868-COL series is sold with two different firmware versions, each with different functionality:
- one using the hex based protocol documented here. This version allows setting rules on the KC868-COL to auto control KC868-H series relays.
- one using a text string based protocol. This version does not allow setting rules on the KC868-COL to auto control KC868-H series relays autonomously. It does still allow manual control of KC868-H series relays through the KC868-COL though!

The source document nor the translation are perfect, but this combined with the freely available source to should hopefully enable a proper frontend  implementation.
