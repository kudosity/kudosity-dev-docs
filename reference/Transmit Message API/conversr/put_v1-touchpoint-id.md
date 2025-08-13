---
title: Update Touchpoint
excerpt: >-
  Updates the properties of an existing touchpoint.


  Supported properties for update depend on staus of touchpoint, and cannot be
  used to update system generated properties such as `id`, `created_at`,
  `updated_at` and `start`.


  | Touchpoint Status | Properites |

  | --- | --- |

  | draft | All properties can be updated |

  | active | All properties can be updated except `type`, `contact_import_id` |

  | cancelled | No properties can be updated |
api:
  file: public-openapi.yaml
  operationId: put_v1-touchpoint-id
hidden: false
---