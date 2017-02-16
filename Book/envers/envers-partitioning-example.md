 ### 21.22. Audit table partitioning example

    <div class="paragraph">

    In order to determine a suitable column for the 'increasing level of relevancy',
    consider a simplified example of a salary registration for an unnamed agency.

    </div>
    <div class="paragraph">

    Currently, the salary table contains the following rows for a certain person X:

    </div>
    <table class="tableblock frame-all grid-all spread">
    <caption class="title">Table 11. Salaries table</caption>
    <colgroup>
    <col style="width: 50%;">
    <col style="width: 50%;">
    </colgroup>
    <thead>
    <tr>
    <th class="tableblock halign-left valign-top">Year</th>
    <th class="tableblock halign-left valign-top">Salary (USD)</th>
    </tr>
    </thead>
    <tbody>
    <tr>
    <td class="tableblock halign-left valign-top">

    2006
    </td>
    <td class="tableblock halign-left valign-top">

    3300
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    2007
    </td>
    <td class="tableblock halign-left valign-top">

    3500
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    2008
    </td>
    <td class="tableblock halign-left valign-top">

    4000
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    2009
    </td>
    <td class="tableblock halign-left valign-top">

    4500
    </td>
    </tr>
    </tbody>
    </table>
    <div class="paragraph">

    The salary for the current fiscal year (2010) is unknown.
    The agency requires that all changes in registered salaries for a fiscal year are recorded (i.e. an audit trail).
    The rationale behind this is that decisions made at a certain date are based on the registered salary at that time.
    And at any time it must be possible reproduce the reason why a certain decision was made at a certain date.

    </div>
    <div class="paragraph">

    The following audit information is available, sorted on in order of occurrence:

    </div>
    <table class="tableblock frame-all grid-all spread">
    <caption class="title">Table 12. Salaries - audit table</caption>
    <colgroup>
    <col style="width: 20%;">
    <col style="width: 20%;">
    <col style="width: 20%;">
    <col style="width: 20%;">
    <col style="width: 20%;">
    </colgroup>
    <thead>
    <tr>
    <th class="tableblock halign-left valign-top">Year</th>
    <th class="tableblock halign-left valign-top">Revision type</th>
    <th class="tableblock halign-left valign-top">Revision timestamp</th>
    <th class="tableblock halign-left valign-top">Salary (USD)</th>
    <th class="tableblock halign-left valign-top">End revision timestamp</th>
    </tr>
    </thead>
    <tbody>
    <tr>
    <td class="tableblock halign-left valign-top">

    2006
    </td>
    <td class="tableblock halign-left valign-top">

    ADD
    </td>
    <td class="tableblock halign-left valign-top">

    2007-04-01
    </td>
    <td class="tableblock halign-left valign-top">

    3300
    </td>
    <td class="tableblock halign-left valign-top">

    null
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    2007
    </td>
    <td class="tableblock halign-left valign-top">

    ADD
    </td>
    <td class="tableblock halign-left valign-top">

    2008-04-01
    </td>
    <td class="tableblock halign-left valign-top">

    35
    </td>
    <td class="tableblock halign-left valign-top">

    2008-04-02
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    2007
    </td>
    <td class="tableblock halign-left valign-top">

    MOD
    </td>
    <td class="tableblock halign-left valign-top">

    2008-04-02
    </td>
    <td class="tableblock halign-left valign-top">

    3500
    </td>
    <td class="tableblock halign-left valign-top">

    null
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    2008
    </td>
    <td class="tableblock halign-left valign-top">

    ADD
    </td>
    <td class="tableblock halign-left valign-top">

    2009-04-01
    </td>
    <td class="tableblock halign-left valign-top">

    3700
    </td>
    <td class="tableblock halign-left valign-top">

    2009-07-01
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    2008
    </td>
    <td class="tableblock halign-left valign-top">

    MOD
    </td>
    <td class="tableblock halign-left valign-top">

    2009-07-01
    </td>
    <td class="tableblock halign-left valign-top">

    4100
    </td>
    <td class="tableblock halign-left valign-top">

    2010-02-01
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    2008
    </td>
    <td class="tableblock halign-left valign-top">

    MOD
    </td>
    <td class="tableblock halign-left valign-top">

    2010-02-01
    </td>
    <td class="tableblock halign-left valign-top">

    4000
    </td>
    <td class="tableblock halign-left valign-top">

    null
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    2009
    </td>
    <td class="tableblock halign-left valign-top">

    ADD
    </td>
    <td class="tableblock halign-left valign-top">

    2010-04-01
    </td>
    <td class="tableblock halign-left valign-top">

    4500
    </td>
    <td class="tableblock halign-left valign-top">

    null
    </td>
    </tr>
    </tbody>
    </table>
    </div>
    <div class="sect2">