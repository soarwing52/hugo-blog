---

title: "AspNet 新增user/role表格"

date: 2021-12-17

description: "AspNet 新增user/role表格"

tags:
  - dotnet
  - Work
  - 中文

---

在用了預設的 identity之後 就會要加欄位了

原本的用法是有userstore/ usermanager rolestore rolemanager

以下是已經改好之後的設定

    

    

            private UserStore<ApplicationUser> userStore;

            private UserManager<ApplicationUser> userManager;

            private RoleStore<ApplicationRole> roleStore;

            private RoleManager<ApplicationRole> roleManager;

    

                userStore = new UserStore<ApplicationUser>(applicationDbContext);

                userManager = new UserManager<ApplicationUser>(userStore);

                userManager.UserValidator = new UserValidator<ApplicationUser>(userManager) { AllowOnlyAlphanumericUserNames = false };

                roleStore = new RoleStore<ApplicationRole>(applicationDbContext);

                roleManager = new RoleManager<ApplicationRole>(roleStore);

    

在model這裡先繼承原本的user跟role

    

    

        public class ApplicationUser : IdentityUser

        {

        }

    

        public class ApplicationRole : IdentityRole

        {

            public string Description { get; set; }

        }

我先單純加了一個description欄位

然後原本的DB要改成用新的USER

    

    

        public class ApplicationDbContext : IdentityDbContext<ApplicationUser>

        {

            public ApplicationDbContext() : base("DefaultConnection")

            {

            }

        }

接下來就是設定STORE 這樣的話就可以把新增了欄位的class給STORE去讀取

    

    

        public class AppUserManager : UserManager<ApplicationUser>

        {

            // 加入ApplicationUserStore，建構函數要傳入 DbContext

            public AppUserManager() : base(new UserStore<ApplicationUser>(new ApplicationDbContext()))

            {

                this.UserValidator = new UserValidator<ApplicationUser>(this)

                {

                    AllowOnlyAlphanumericUserNames = false

                };

            }

        }

    

另外還有一種作法

    

    

        public class AppRoleStore : RoleStore<ApplicationRole>

        {

            public AppRoleStore() : base(new ApplicationDbContext()) { }

        }

    

        public class AppRoleManager : RoleManager<ApplicationRole>

        {

            public AppRoleManager(): base(new AppRoleStore()) { }

        }

這樣子只要用 new AppRoleManager()就可以直接使用 不需要再叫出store囉

這邊要注意的一點，就是繼承之後 add-migration出現了一個叫做Discriminator的欄位

一開始我不以為意，然後我的manager一直找不到role

後來才發現 那個欄位就會寫 AppplicationRole代表了他所寫入的class名稱

因為這個我卡了半天，調整各種設定，結果只是因為我沒有把這個欄位補上，補上就可以正常使用了

