exports.views = {
    by_itag: {
        map: function (doc) {
            if(doc.itag) {
               emit(doc.itag,null);
            }
        }
    },by_macid: {
        map: function (doc) {
            if(doc.macid) {
               emit(doc.macid,null);
            }
        }
    },
    by_racklocation: {
        map: function (doc) {
            if(doc.rack) {
               emit(doc.rack, null);
            }
        }
    },
    by_switchname: {
        map: function (doc) {
            if(doc.switchname) {
               emit(doc.switchname, null);
            }
        }
    },
    by_vendor: {
        map: function (doc) {
            if(doc.vendor) {
               emit(doc.vendor, null);
            }
        }
    },
    by_servername: {
        map: function (doc) {
            if(doc.servername) {
               emit(doc.servername, null);
            }
        }
    },
    by_ipaddress: {
        map: function (doc) {
            if(doc.ipaddress) {
               emit(doc.ipaddress, null);
            }
        }
    },
    servers_owned_by_team_grid: {
        map: function (doc) {
            if(doc.owner.indexOf("Jabir") >= 0) {
               emit(doc.servername, null);
            }
        }
    },
    servers_owned_by_team_adserve: {
        map: function (doc) {
            if(doc.owner.indexOf("Mazin") >= 0) {
               emit(doc.servername, null);
            }
        }
    },
    servers_owned_by_team: {
        map: function (doc) {
            if(doc.project) {
               emit(doc.project, null);
            }
        }
    },
      by_substring: {
        map: function (doc) {
                 var stringarray = doc.title.split(" ");
         for(var idx in stringarray)
         emit(stringarray[idx],doc);
        }
    },
 by_processor_speed: {
    map: function (doc) {
      if (doc._id.indexOf("asset.server") >= 0 || doc._id.indexOf("asset.ec2") >= 0) {
        if (doc.processor) {
          emit(doc.processor.sp_current_processor_speed, doc.other.fqdn);
        }
      }
    }
  },
  by_processor_model: {
    map: function (doc) {
      if (doc._id.indexOf("asset.server") >= 0 || doc._id.indexOf("asset.ec2") >= 0) {
        for(k in doc.processor) {
          if(k.match(".*[0-9]$")) {
            emit(doc.processor[k], doc.other.fqdn);
          }
        }
      }
    }
  },
  by_processor_count: {
    map: function (doc) {
      if (doc._id.indexOf("asset.server") >= 0 || doc._id.indexOf("asset.ec2") >= 0) {
        if (doc.processor) {
          emit(doc.processor.processorcount, doc.other.fqdn);
        }
      }
    }
  },
  by_memory: {
    map: function (doc) {
      if (doc._id.indexOf("asset.server") >= 0 || doc._id.indexOf("asset.ec2") >= 0) {
        if(doc.memory) {
          emit(doc.memory.memorytotal, doc.other.fqdn);
        }
      }
    }
  },
  by_os: {
    map: function (doc) {
      if (doc._id.indexOf("asset.server") >= 0 || doc._id.indexOf("asset.ec2") >= 0) { 
        if(doc.os.operatingsystem) {
          emit(doc.os.operatingsystem, doc.other.fqdn);
        }
      } 
    }
  },
  by_kernel: {
    map: function(doc) {
      if(doc._id.indexOf("asset.server") >= 0 || doc._id.indexOf("asset.ec2") >= 0) {
        for (attr in doc.kernel) {
          emit(doc.kernel[attr], doc.other.fqdn);
        }
      }
    }
  },
   all_attributes: {
    map: function(doc) {
      if(doc._id.indexOf("asset.server") >= 0) {
        for (k in doc) {
          emit( k,null);
        }
      }
    },
    reduce: function(keys, values, reduce) {
      return true;
    }
  },
all_attr: {
    map: function(doc) {
      function recur(subdoc,prefix) {
        for (var p in subdoc)
        {   
          var obj_type = Object.prototype.toString.call(doc[p]);
          if ( obj_type == "[object Object]") {
            if(prefix == null) 
              prefix = p;
            else 
              recur(subdoc[p],prefix+'.'+p);
          }   
          else
            if (prefix == undefined) 
              prefix = ""; 
            emit(prefix+'.'+p, subdoc[p]);
        }   
      }   
      recur(doc);
    }, 
    reduce: function(key, values, reduce) {
      return true;
    }   
  },
all_attr_type: {
    map: function(doc) {
/*      if(doc._id.indexOf("asset.server") >= 0 || doc._id.indexOf("asset.storage") >= 0 || doc._id.indexOf("asset.ec2") >= 0) { */
      if(doc._id.match("^asset.*")) {   
    
        var asset_type;
        if(doc._id.indexOf("asset.server") >= 0 ) { asset_type = "Server" }
        if(doc._id.indexOf("asset.ec2") >= 0 ) { asset_type = "EC2" }
        if(doc._id.indexOf("asset.network") >= 0 ) { asset_type = "Network" }
        if(doc._id.indexOf("asset.storage") >= 0 ) { asset_type = "Storage" }
        if(doc._id.indexOf("asset.cluster") >= 0 ) { asset_type = "Cluster" }
        if(doc._id.indexOf("asset.service") >= 0 ) { asset_type = "Service" }
        if(doc._id.indexOf("asset.environment") >= 0 ) { asset_type = "Environment" }
        if(doc._id.indexOf("asset.host") >= 0 ) { asset_type = "Host" }

        function recur(subdoc,prefix) {
          for (var p in subdoc)
          {   
            var obj_type = Object.prototype.toString.call(doc[p]);
            if ( obj_type == "[object Object]") {
              if(prefix == null) 
                prefix = p;
              else 
                /*prefix = prefix+'.'+p;*/
                recur(subdoc[p],prefix+'.'+p);
            }   
            else
              if (prefix == undefined) 
                prefix = ""; 
                var attr_data_type = typeof p;
                emit(asset_type, prefix+'.'+p);
          }   
        }
	   
        recur(doc);
      }   
    },  
    reduce: function(key, values, reduce) {
      return true;
    }
},

attr_type: {
    map: function(doc) {
/*      if(doc._id.indexOf("asset.server") >= 0 || doc._id.indexOf("asset.storage") >= 0 || doc._id.indexOf("asset.ec2") >= 0) { */
        var asset_type;
      if(doc._id.match("^asset.*")) {   
    

        function recur(subdoc,prefix) {
          for (var p in subdoc)
          {   
            var obj_type = Object.prototype.toString.call(doc[p]);
            if ( obj_type == "[object Object]") {
              if(prefix == null) 
                prefix = p;
              else 
                /*prefix = prefix+'.'+p;*/
                recur(subdoc[p],prefix+'.'+p);
            }   
            else
              if (prefix == undefined) 
                prefix = ""; 
                var attr_data_type = typeof p;
                emit(asset_type, prefix+'.'+p);
          }   
        }
        if(doc._id.indexOf("asset.server") >= 0 ) { asset_type = "Server";recur(doc); }
        if(doc._id.indexOf("asset.ec2") >= 0 ) { asset_type = "EC2";recur(doc); }
        if(doc._id.indexOf("asset.network") >= 0 ) { asset_type = "Network";recur(doc); }
        if(doc._id.indexOf("asset.storage") >= 0 ) { asset_type = "Storage";recur(doc); }
        if(doc._id.indexOf("asset.cluster") >= 0 ) { asset_type = "Cluster";recur(doc); }
        if(doc._id.indexOf("asset.service") >= 0 ) { asset_type = "Service";recur(doc); }
        if(doc._id.indexOf("asset.environment") >= 0 ) { asset_type = "Environment";recur(doc); }
        if(doc._id.indexOf("asset.host") >= 0 ) { asset_type = "Host";recur(doc); }
	   
        
      }   
    }
 },  
 by_type: {
        map: function (doc) {
           if(doc._id.indexOf("asset.type_doc") >= 0) {
               emit(doc.name, doc.doc_text);
            }
        }
    },

 by_attr: {
        map: function (doc) {
           if(doc._id.indexOf("asset.attr_doc") >= 0) {
               emit(doc.value, doc);
            }
        }
    }
};
