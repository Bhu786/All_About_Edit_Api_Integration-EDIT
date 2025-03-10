# All_About_Edit_Api_Integration-EDIT



##### step 1:

```js
 <Button
            onClick={() => openEditModalPopup(row._id)}
            variant="outlined"
          >
            Edit
          </Button>
```

##### step 2:

```js

 const openEditModalPopup = (userId: string) => {
    // check id aaya ki nhi edit button see
    console.log("Selected User ID:", userId); // Debugging

    setSelectedUserId(userId);

      // edit button click karne par popup form open hoga uske leye tha 
    setShowModal(true);

    // call karo fun ko id pass kar ki 
    fetchUserDetails(userId);
  };

```

##### id ko bhi set kara do func me 
```js
const [selectedUserId, setSelectedUserId] = useState<string | null>(null);
````


###### fun bnao api call with id 
```js

const fetchUserDetails = async (userId: string) => {
  const url = `admin/API_User_Management/getApiUserDetails/${userId}`;
  const token = "your-auth-token"; // Replace with actual token if needed
  const body = {}; // No body for a GET request

 Api(url, "GET", body, token)
.then((Response: any) => {
           console.log("====User Details API Response====>", Response);

      if (Response?.status === 200 && Response.data?.code === 200) {
        console.log("User Details Fetched:", Response.data.data);
        setUserData({
          firstName: Response.data.data.firstName || "",
          lastName: Response.data.data.lastName || "",
          contact_no: Response.data.data.contact_no || "",
          userCode: Response.data.data.userCode || "",
          email: Response.data.data.email || "",
          GSTNumber: Response.data.data.GSTNumber || "",
          company_name: Response.data.data.company_name || "",
          AEPS_wallet_amount: Response.data.data.AEPS_wallet_amount || 0,
          main_wallet_amount: Response.data.data.main_wallet_amount || 0,
          bin_verify_charges: Response.data.data.bin_verify_charges || 0,
          beneValidationCharge: Response.data.data.beneValidationCharge || 0,
          upiValidationCharge: Response.data.data.upiValidationCharge || 0,
        });
      } else {
        enqueueSnackbar(Response.data?.message || "Error fetching user details", { variant: "error" });
      }
    })
 .catch((error: any) => {
      console.error("API Error:", error);
      enqueueSnackbar("API request failed", { variant: "error" });
    });
};
```




###### save or update karana

==>
```
 <Button
                      variant="contained"
                      color="primary"
                      onClick={handleSaveedit}
                    >
                      Save
                    </Button>

```

#### 2nd button hit hone par chelega yehh for save or updated ki leye

```

// Now Save
const handleSaveedit = async () => {
  if (!selectedUserId) {
    console.error("User ID is missing");
    return;
  }

  const url = `admin/API_User_Management/updateApiUserDetails/${selectedUserId}`;
  const token = "your-auth-token"; // Replace with actual token if needed

  Api(url, "POST", userData, token)
    .then((Response: any) => {
      console.log("====Update User API Response====>", Response);

      if (Response?.status === 200 && Response.data?.code === 200) {
        console.log("User Updated Successfully:", Response.data);
        enqueueSnackbar("User updated successfully!", { variant: "success" });

        // Fetch updated user details immediately after saving
        fetchUserDetails(selectedUserId);

        closeModal(); // Close modal after saving
      } else {
        enqueueSnackbar(Response.data?.message || "Failed to update user", { variant: "error" });
      }
    })
    .catch((error: any) => {
      console.error("API Error:", error);
      enqueueSnackbar("API request failed", { variant: "error" });
    });
    
};
```



