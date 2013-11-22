function GetCursorLocation(CurrentTextBox) {
    var CurrentSelection, FullRange, SelectedRange, LocationIndex = -1;
    if (typeof CurrentTextBox.selectionStart == "number") {
        LocationIndex = CurrentTextBox.selectionStart;
    } else if (document.selection && CurrentTextBox.createTextRange) {
        CurrentSelection = document.selection;
        if (CurrentSelection) {
            SelectedRange = CurrentSelection.createRange();
            FullRange = CurrentTextBox.createTextRange();
            FullRange.setEndPoint("EndToStart", SelectedRange);
            LocationIndex = FullRange.text.length;
        }
    }
    return LocationIndex;
}

function cursor_actions(attr_array) {
    $("input").keypress(function () {
        c = GetCursorLocation(this);
        val = $("#search").val();
        cursor = (val[c - 1]);

        var str = val.slice(0, c - 1);
        if (cursor == undefined || cursor == "" || cursor == "(") {
            auto_unique_attr(attr_array);
        } else if (cursor == "=" || cursor == ">" || cursor == "<") {
            auto_attr_value(str);
        } else if (lastWord(this.value) == "AND" || lastWord(this.value) == "OR" || lastWord(this.value) == "and" || lastWord(this.value) == "or") {
            auto_unique_attr(attr_array);
        } else if (cursor == " " && lastWord(this.value).indexOf("(") == -1 && (lastWord(this.value).indexOf("=") != -1 ||
            lastWord(this.value).indexOf("<") != -1 || lastWord(this.value).indexOf(">") != -1 || lastWord(this.value).indexOf("GHz") != -1)) {
            auto_operator(lastWord(this.value));
        }
    });
}

function get_asset_attributes(url) {
    $.ajax({
        type: 'GET',
        headers: {
            'Content-Type': "application/json"
        },
        url: url,
        success: function (data) {
            data = JSON.parse(data);
            attr_array = [];
            var keys = [];
            attr_array[0] = "(";
            attr_array[1] = ")";
            j = 2;
            for (i = 0; i < data.rows.length; i++) {
                if (data.rows[i].key[1].indexOf("_id") == -1 && data.rows[i].key[1].indexOf("_rev") == -1 && data.rows[i].key[1].indexOf(".doc") == -1  && data.rows[i].key[1].indexOf(".type") == -1) {
                    attr_array[j] = data.rows[i].key[1];
                    ++j;
                }
            }
            cursor_actions(attr_array);
        }
    });
}
function form_functions()
{

 $("form").keyup(function (event) {
                     collection_api();
                     event.preventDefault();
                 });
 $("form").submit(function (event) {
                     collection_api();
                     event.preventDefault();
                 });
$("#b1").click(function (event) {
                    $("#search").val("");$("#content").val("");
                 });

}

function load_default() {
    $("#search_query").empty();
 
    placeholder = "server";
   
    url = 'http://ims.corp.inmobi.com:5984/ims/_design/sa_reports/_view/all_attributes?group=true&reduce=true&startkey=["network",{}]&endkey=["server",{}]';


    get_asset_attributes(url);
 $("#search_query").append("<form  ><input type='text' id='search' name='search' autocomplete='off' size='100' placeholder=" + placeholder + "></form><button id='b1' class='btn btn-primary' type='button'>Clear</button>");
    $("#search").popover({
        trigger: 'focus',
        title: 'Write IMS Search query',
        content: "Example:itag=UA2SR20342 OR (macid>03:04:a3:4c:44:0c OR colo=ua1)"
    });
    form_functions();

}

function get_asset(data) {
    asset_array = [];
    data = JSON.parse(data);
    asset_description = {};
    for (i = 0; i < data.rows.length; i++) {
        str = data.rows[i].key.toUpperCase();
        $(".dropdown-menu").append("<li><a href='#'>" + str + "</a></li>");
        asset_description[data.rows[i].key] = data.rows[i].value;
        asset_array[i] = data.rows[i].key;
    }
}

function create_url(data) {
    asset_array = [];
    asset_description = {};
    for (i = 0; i < data.rows.length; i++) {
        asset_description[data.rows[i].key] = data.rows[i].value;
        asset_array[i] = data.rows[i].key;
    }
    for (i = 0; i < asset_array.length; i++) {
        if (asset_array[i] == asset) {
            k = i - 1;
            break;
        }
    }
    endkey = [];
    startkey = [];
    if (k == -1) {
        endkey.push(asset_array[k + 1]);
        endkey.push({});
        startkey = [];
    } else {
        startkey.push(asset_array[k]);
        startkey.push({});
        endkey.push(asset_array[k + 1]);
        endkey.push({});
    }
    startkey = JSON.stringify(startkey);
    endkey = JSON.stringify(endkey);
}
var lastWord = function (o) {
    return ("" + o).replace(/[\s-]+$/, '').split(/[\s-]/).pop();
};

function findmatch(term, attr_array) {
    var term = term.toLowerCase();
    var matchingTags = $.grep(attr_array, function (tag) {
        return tag.toLowerCase().indexOf(term) >= 0;
    });
    return matchingTags;
}

function auto_unique_attr(attr_array) {
    $("#search").autocomplete({
        minLength: 0,
        source: function (request, response) {
            var term = request.term,
                results = [];
            if (lastWord(term).indexOf("(") != -1) {
                term = extractLast_br(term);
            } else {
                term = extractLast3(term);
            }
            if (term.length > 0) {
                var matcher = new RegExp("^" + $.ui.autocomplete.escapeRegex(term), "i");
                var matchingTags = ($.grep(attr_array, function (item) {
                    return matcher.test(item);
                }));
                response((matchingTags.length ? matchingTags : matchingTags));
            } else {
                results = attr_array;
                response(results);
            }
        },
        focus: function () {
            return false;
        },
        select: function (event, ui) {
            if (lastWord(this.value).indexOf("(") != -1) {
                var terms = split_br(this.value);
                terms.pop();
                terms = terms + "(";
            } else {
                var terms = split3(this.value);
                terms.pop();
            }
            value = $("#search").val();
            if (terms != "")
                var a = terms + "  " + ui.item.value
            else var a = ui.item.value
            a.replace(/\,/g, "  ");
            var a = a.replace(/\,/g, "  ");
            if (a.slice(-1) != "(") {
                this.value = a;
                auto_attr_value(a);
            } else {
                this.value = a;
            }
            val = $("#search").val();
            //auto_ops(val);
            return false;
        }
    });
}

function auto_ops(val) {
    var opr_array = ["=", "<", ">"];
    $("#search").bind("keydown", function (event) {
        if (event.keyCode === $.ui.keyCode.TAB && $(this).data("autocomplete").menu.active) {
            event.preventDefault();
        }
    }).autocomplete({
        minLength: 0,
        source: function (request, response) {
            var term = request.term,
                results = [];
            if (lastWord(term).indexOf("=") == -1) {
                results = opr_array;
                response(results);
            } else {
                auto_attr_value(this.value);
            }
        },
        focus: function () {
            return false;
        },
        select: function (event, ui) {
            this.value = val + ui.item.value;
            auto_unique_attr(this.value)
            return false;
        }
    });
}

function get_attr_values(str) {
    value = lastWord(str);
    $.getJSON("url_json.json", function (result) {
        url = result["unique_attr_url"];
        unique_attr_url = url + "&&key=%22." + value + "%22";
        $.ajax({
            type: 'GET',
            headers: {
                'Content-Type': "application/json"
            },
            url: unique_attr_url,
            success: function (data) {
                data = JSON.parse(data);
                attribute_array = [];
                j = 0;
                for (i = 0; i < data.rows.length; i++) {
                    var attr = String(data.rows[i].value);
                    if (attribute_array.indexOf(attr) == -1 && typeof data.rows[i].value != "object") {
                        attribute_array[j] = attr;
                        ++j;
                    }
                }
                return (attribute_array);
            }
        });
    });
}

function auto_attr_value(str) {
    get_attr_values(str);
    //autosearch for attribute values
    $("#search")
        .bind("keydown", function (event) {
            if (event.keyCode === $.ui.keyCode.TAB && $(this).data("autocomplete").menu.active) {
                event.preventDefault();
            }
        }).autocomplete({
            minLength: 0,
            source: function (request, response) {
                var term = request.term;
                results = [];
                if (lastWord(term).indexOf("=") >= 0) {
                    term = extractLast1(request.term);
                    results = findmatch(term, attribute_array);
                } else if (lastWord(term).indexOf(">") >= 0) {
                    term = extractLast_gt(request.term);
                    results = findmatch(term, attribute_array);
                } else if (lastWord(term).indexOf("<") >= 0) {
                    term = extractLast_lt(request.term);
                    results = findmatch(term, attribute_array);
                }
                response(results.slice(0, 15));
            },
            focus: function (event, ui) {
                    str = lastWord(this.value);
               text_str=focus_attribute_values(this.value,ui.item.value);
                var strLen = text_str.length;
                text_str = text_str.slice(0, strLen - 1);
                this.value = text_str + " ";
                collection_api();
                return false;
              
            },
            select: function (event, ui) {
                 str = lastWord(this.value);
               text_str=focus_attribute_values(this.value,ui.item.value);
                var strLen = text_str.length;
                text_str = text_str.slice(0, strLen - 1);
                this.value = text_str + " ";
                collection_api();
                return false;
            }
        }).bind('focus', function () {
            $(this).autocomplete("search");
        });
}

function focus_attribute_values(str,item)
{
if (lastWord(str).indexOf("=") >= 0) {
                    var terms = split1(str);
                    terms.pop();
                    terms.push(item);
                    terms.push("");
                    var text_str = terms.join("=");
                }
                if (lastWord(str).indexOf("<") >= 0) {
                    var terms = split_lt(this.value);
                    terms.pop();
                    terms.push(item);
                    terms.push("");
                    var text_str = terms.join("<");
                }
                if (lastWord(str).indexOf(">") >= 0) {
                    var terms = split_gt(this.value);
                    terms.pop();
                    terms.push(item);
                    terms.push("");
                    var text_str = terms.join(">");
                }
return text_str;
}
function collection_api() {
    console.log("collection_api");
    val = $("#search").val();
    val = val.replace(/ AND /gi, "%26");
    val = val.replace(/ OR /gi, "|");
 val = val.replace(/ and /gi, "%26");
    val = val.replace(/ or /gi, "|");
    url = 'http://ims.corp.inmobi.com:5984/ims/_design/collection/_list/collection/collection?ex=' + val;       +"&filter=macid,servername,physical,itag,owner,macid,switch";

    url = url.replace(/ /g, '');
    url = url.replace(/ /g, '');
    $.ajax({
        type: 'GET',
        headers: {
            'Content-Type': "application/json"
        },
        url: url,
        success: function (data) {
            $("#content").empty();
            $("#content").append(JSON.stringify(data));
        }
    });
}

function auto_operator() {
    var operators_array = [")", "CONTAINS", "IN", "AND", "OR"];
    $("#search").bind("keydown", function (event) {
        if (event.keyCode === $.ui.keyCode.TAB && $(this).data("autocomplete").menu.active) {
            event.preventDefault();
        }
    }).autocomplete({
        minLength: 0,
        source: function (request, response) {
            var term = request.term,
                results = [];
            if (term.indexOf(" ") >= 0) {
                term = extractLast3(request.term);
                if (term.length > 0) {
                    matchingTags = findmatch(term, operators_array)
                    response((matchingTags.length ? operators_array : operators_array).slice(0, 10));
                } else {
                    results = operators_array;
                    response(results);
                }
            }
        },
        focus: function () {
            
            return false;
        },
        select: function (event, ui) {
            var terms = split3(this.value);
            terms.pop();
            var a = terms + "  " + ui.item.value + "  ";
            a.replace(/\,/g, "  ");
            var a = a.replace(/\,/g, "  ");
            this.value = a;
            return false;
        }
    });
}

function split1(val) {
    return val.split(/\=\s*/);
}

function extractLast1(term) {
    return split1(term).pop();
}

function extractLast_lt(term) {
    return split_lt(term).pop();
}

function extractLast_gt(term) {
    return split_gt(term).pop();
}

function split_lt(val) {
    return val.split(/\<\s*/);
}

function split_gt(val) {
    return val.split(/\>\s*/);
}

function split3(val) {
    return val.split(/ \s*/);
}

function split_br(val) {
    return val.split(/\(\s*/);
}

function extractLast3(term) {
    return split3(term).pop();
}

function extractLast_br(term) {
    return split_br(term).pop();
}
