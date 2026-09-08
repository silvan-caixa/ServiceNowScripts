# ServiceNowScripts

function onChange(control, oldValue, newValue, isLoading, isTemplate) {
    if (isLoading) {
        return;
    }

    g_form.getReference('cmdb_ci', checkName);

    function checkName(ci) {
        var name = ci.name + '';

        // Check the name of the CI using regular expressions;
        // Set and lock Item type field based on the CI name.

        if (name.match(/blackberry.*/i) ||
            name.match(/iphone.*/i) ||
            name.match(/android.*/i)) {

            g_form.setValue('item_type', 'cmdb_ci_mobile_device', 'Mobile Phone');
            g_form.setReadOnly('item_type', true);

        } else if (name.match(/macbook.*/i)) {

            g_form.setValue('item_type', 'cmdb_ci_computer', 'Laptop');
            g_form.setReadOnly('item_type', true);

        } else {

            g_form.setReadOnly('item_type', false);
        }
    }
}