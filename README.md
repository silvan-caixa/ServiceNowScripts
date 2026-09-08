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



# SetScratchpadValues

(function executeRule(current, previous /*null when async*/) {

    g_scratchpad.city = current.requested_for.city;
    g_scratchpad.country = current.requested_for.country;

    // Hooray for dotwalking!
    // By writing these values to the scratchpad, a client script can easily fetch them.

})(current, previous);

# SetLoanerLocation
function onLoad() {

    // This uses the dotwalking superpowers of the Scratchpad to use the requestor's city & state as the default location to be used.
    // Make sure whoever you use for testing has values in their user record for this!

    if (g_form.getValue('location_to_be_used') != '') {
        return;
    }

    var city = g_scratchpad.city;
    var country = g_scratchpad.country;

    if (city && country) {
        g_form.setValue('location_to_be_used', city + ', ' + country);
    } else if (city) {
        g_form.setValue('location_to_be_used', city);
    } else if (country) {
        g_form.setValue('location_to_be_used', country);
    }

    if (city || country) {
        g_form.showFieldMsg(
            'location_to_be_used',
            'Value set automatically - you may override by editing',
            'info'
        );
    }
}

