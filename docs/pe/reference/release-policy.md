---
layout: docwithnav-pe
title: ThingsBoard PE Release Policy
description: ThingsBoard release policy

---

<!-- 
Planned badges are managed manually
EOL badges apply automatically based on patches until date
-->

<style>
    .version-badge {
        display: inline-block;
        white-space: nowrap;
        color: black;
        border: 2px solid black;
        padding: 3px 8px;
        border-radius: 5px;
        font-family: sans-serif;
        font-size: 0.9em;
        font-weight: 600;
        margin-left: 5px;   /* only when on same line */
    }

    .eol-badge {
        background-color: rgb(225, 96, 109);
    }

    .planned-badge {
        background-color: rgb(255, 255, 255);
    }

    .version-badge.eol-badge::after {
        content: " EOL";
    }

    .version-badge.planned-badge::after {
        content: " Planned";
    }

    @media (max-width: 600px) {
        .version-badge {
            margin-left: 0;   /* no gap when wrapped */
            font-size: 7px;
            padding: 2px 2px;
        }

    #docsContent .release-table th,
    #docsContent .release-table td {
        font-size: 9px !important;
        padding: 4px 6px !important;
        white-space: nowrap;
    }
    }
</style>

<script>
document.addEventListener('DOMContentLoaded', function () {
  const today = new Date();

  document.querySelectorAll('.release-table tbody tr').forEach(row => {
    const patchCell = row.cells[4];                  // "Patches until"
    if (!patchCell) return;

    const txt = patchCell.textContent.trim();
    if (txt === '-' || txt === '') return;           // no date → nothing to do

    const patchDate = new Date(txt);
    if (isNaN(patchDate)) return;                   // invalid date → skip

    const isEOL = patchDate < today;
    let badge = row.querySelector('.version-badge');

    if (isEOL) {
      // add or update the EOL badge
      if (!badge) {
        badge = document.createElement('span');
        badge.className = 'version-badge eol-badge';
        row.cells[0].appendChild(document.createTextNode(' '));
        row.cells[0].appendChild(badge);
      } else {
        badge.classList.add('eol-badge');
        badge.classList.remove('planned-badge');
      }
    } else {
      // not EOL → remove any badge that was auto‑inserted earlier
      if (badge && badge.classList.contains('eol-badge')) {
        badge.remove();
      }
      // leave existing Planned badges untouched
    }
  });
});
</script>

This page provides a comprehensive overview of our product's release policy. Here you will find the official release cycle, which lists currently supported, upcoming, and discontinued versions. 

## Release Cycle

<table class="release-table">
    <thead>
        <tr>
            <th>Version</th>
            <th>Latest</th>
            <th>First Release</th>
            <th>Hotfixes until</th>
            <th>Patches until</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>
                4.3.x
                <span class="version-badge planned-badge"></span>
            </td>
            <td>-</td>
            <td>Dec 2025</td>
            <td>-</td>
            <td>-</td>
        </tr>
        <tr>
            <td>
                4.2.x
                <span class="version-badge planned-badge"></span>
            </td>
            <td>-</td>
            <td>Sep 2025</td>
            <td>-</td>
            <td>-</td>
        </tr>
        <tr>
            <td>4.1.x</td>
            <td>4.1.0</td>
            <td>Jul 3, 2025</td>
            <td>Jan 3, 2026</td>
            <td>Jan 3, 2027</td>
        </tr>
        <tr>
            <td>4.0.x</td>
            <td>4.0.2</td>
            <td>Apr 15, 2025</td>
            <td>Oct 15, 2025</td>
            <td>Oct 15, 2026</td>
        </tr>
        <tr>
            <td>3.9.x</td>
            <td>3.9.1</td>
            <td>Dec 31, 2024</td>
            <td>Jun 30, 2025</td>
            <td>Jun 30, 2026</td>
        </tr>
        <tr>
            <td>3.8.x</td>
            <td>3.8.1</td>
            <td>Oct 3, 2024</td>
            <td>Apr 3, 2025</td>
            <td>Apr 3, 2026</td>
        </tr>
        <tr>
            <td>3.7.x</td>
            <td>3.7.0</td>
            <td>Jun 17, 2024</td>
            <td>Dec 17, 2024</td>
            <td>Dec 17, 2025</td>
        </tr>
        <tr>
            <td>
                3.6.x
            </td>
            <td>3.6.4</td>
            <td>Sep 21, 2023</td>
            <td>Mar 21, 2024</td>
            <td>Mar 21, 2025</td>
        </tr>
        <tr>
            <td>
                3.5.x
            </td>
            <td>3.5.1</td>
            <td>May 9, 2023</td>
            <td>Nov 9, 2023</td>
            <td>Nov 9, 2024</td>
        </tr>
    </tbody>
</table>

### Release Statuses

* **Active:** A version is considered Active if it is not marked as `Planned` or `EOL`. Active versions are currently supported according to the policy outlined below.
* **Planned:** A `Planned` version is an upcoming release that is under development. The provided release dates are estimates and subject to change.
* **EOL (End of Life):** A version is designated as End of Life (EOL) when its defined support period has concluded. EOL versions are no longer eligible for software updates or technical support. Users are strongly encouraged to upgrade from any version that has reached its End of Life.

### Support Phases for Active Releases

Each active version series receives support for a total of **1.5 years (18 months)**, which is split into two distinct phases:

1.  **Hotfix Phase (First 6 months)**
    For the first **6 months** following its initial release, a version receives comprehensive updates. This includes both critical security patches and regular hotfixes to address bugs and stability issues. The end of this period is indicated in the **"Hotfixes until"** column.

2.  **Patch Phase (Next 12 months)**
    For the **12 months** following the end of the Hotfix Phase, a version receives critical security patches only. No new hotfixes for non-security-related bugs will be provided during this time. The end of this period, shown in the **"Patches until"** column, marks the version's End of Life.

Support for all Active versions includes community-level assistance via [GitHub issues](https://github.com/thingsboard/thingsboard/issues) and access to the [ThingsBoard Support Portal](https://thingsboard-portal.atlassian.net/servicedesk) for customers with an appropriate subscription plan.
