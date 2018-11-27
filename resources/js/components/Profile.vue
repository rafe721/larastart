<style>
    .widget-user-header {
        background-position: center center;
        background-size: cover;
        height: 250px !important;
    }
    input[type='file'] {

        border: 0 none;
    }
</style>

<template>
    <div class="container">
        <div class="row">
            <div class="col-md-12 mt-3">
                <div class="card card-widget widget-user">
                    <!-- Add the bg color to the header using any of the bg-* classes -->
                    <div class="widget-user-header text-white" style="background-image: url('./img/user-cover.png'">
                        <h3 class="widget-user-username">Elizabeth Pierce</h3>
                        <h5 class="widget-user-desc">Web Designer</h5>
                    </div>
                    <div class="widget-user-image">
                        <img class="img-circle" :src="updateProfilePic()" alt="User Avatar">
                    </div>
                    <div class="card-footer">
                        <div class="row">
                            <div class="col-sm-4 border-right">
                                <div class="description-block">
                                    <h5 class="description-header">3,200</h5>
                                    <span class="description-text">SALES</span>
                                </div>
                                <!-- /.description-block -->
                            </div>
                            <!-- /.col -->
                            <div class="col-sm-4 border-right">
                                <div class="description-block">
                                    <h5 class="description-header">13,000</h5>
                                    <span class="description-text">FOLLOWERS</span>
                                </div>
                                <!-- /.description-block -->
                            </div>
                            <!-- /.col -->
                            <div class="col-sm-4">
                                <div class="description-block">
                                    <h5 class="description-header">35</h5>
                                    <span class="description-text">PRODUCTS</span>
                                </div>
                                <!-- /.description-block -->
                            </div>
                            <!-- /.col -->
                        </div>
                        <!-- /.row -->
                    </div>
                </div>
                <div class="card">
                    <div class="card-header p-2">
                        <ul class="nav nav-pills">
                            <li class="nav-item"><a class="nav-link" href="#activity" data-toggle="tab">Activity</a></li>
                            <li class="nav-item"><a class="nav-link active show" href="#settings" data-toggle="tab">Settings</a></li>
                        </ul>
                    </div><!-- /.card-header -->
                    <div class="card-body">
                        <div class="tab-content">
                            <div class="tab-pane" id="activity">
                                <h1 style="text-align: center;">Display User Activity</h1>
                            </div>
                            <!-- /.tab-pane -->

                            <div class="tab-pane active show" id="settings">
                                <form class="form-horizontal">
                                    <div class="form-group">
                                        <label for="inputName" class="col-sm-2 control-label">Name</label>

                                        <div class="col-sm-10">
                                            <input type="text"
                                                   v-model="form.name"
                                                   class="form-control" id="inputName"
                                                   placeholder="Name"
                                                   :class="{ 'is-invalid': form.errors.has('name') }"
                                                >
                                            <has-error :form="form" field="name"></has-error>
                                        </div>
                                    </div>
                                    <div class="form-group">
                                        <label for="inputEmail" class="col-sm-2 control-label">Email</label>

                                        <div class="col-sm-10">
                                            <input type="email" v-model="form.email"
                                                   class="form-control" id="inputEmail"
                                                   placeholder="Email"
                                                   :class="{ 'is-invalid': form.errors.has('email') }"
                                                >
                                            <has-error :form="form" field="email"></has-error>
                                        </div>
                                    </div>
                                    <div class="form-group">
                                        <label for="inputExperience" class="col-sm-2 control-label">Experience</label>

                                        <div class="col-sm-10">
                                            <textarea v-model="form.bio"
                                                      class="form-control"
                                                      id="inputExperience"
                                                      placeholder="Bio"
                                                      :class="{ 'is-invalid': form.errors.has('bio') }"></textarea>
                                            <has-error :form="form" field="bio"></has-error>
                                        </div>
                                    </div>
                                    <div class="form-group">
                                        <label for="inputProfilePhoto" class="col-sm-2 control-label">Profile Photo</label>

                                        <div class="col-sm-10">
                                            <input type="file" class="form-control" @change="updateProfile" name="photo" id="inputProfilePhoto" placeholder="Name">
                                        </div>
                                    </div>
                                    <div class="form-group">
                                        <label for="inputPassport" class="col-sm-6 control-label">Passport (leave empty if not changing)</label>

                                        <div class="col-sm-10">
                                            <input type="password"
                                                    class="form-control" v-model="form.password"
                                                    id="inputPassport" placeholder="Passport"
                                                   :class="{ 'is-invalid': form.errors.has('password') }">
                                            <has-error :form="form" field="password"></has-error>
                                        </div>
                                    </div>
                                    <div class="form-group">
                                        <div class="col-sm-offset-2 col-sm-10">
                                            <button type="submit" @click.prevent="updateInfo" class="btn btn-success">Update</button>
                                        </div>
                                    </div>
                                </form>
                            </div>
                            <!-- /.tab-pane -->
                        </div>
                        <!-- /.tab-content -->
                    </div><!-- /.card-body -->
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                form : new Form({
                    id : '',
                    name : '',
                    email : '',
                    password : '',
                    type : '',
                    bio : '',
                    photo : "",
                })
            }
        },
        mounted() {
            console.log('Component mounted.')
        },
        methods : {
            updateProfilePic() {
                let image_url = 'img/profile/' + this.form.photo;
                if (this.form.photo.includes("base64")) {
                    return this.form.photo;
                } else {
                    return image_url;
                }
            },
            loadProfile() {
                axios.get('api/profile')
                    .then(({ data }) => { this.form.fill(data); });
            },
            updateInfo(e) {
                this.$Progress.start();
                this.form.put('api/profile')
                    .then(() => {
                        this.$Progress.finish();
                        toast({
                            type: 'success',
                            title: 'Profile Updated successfully'
                        })
                    })
                    .catch(() => {
                        this.$Progress.fail();
                    });
            },
            updateProfile(e) {
                var file = e.target.files[0];
                var reader  = new FileReader();
                if (file['size'] < 2111775) {
                    reader.onloadend = () => {
                        console.log("Result: " + reader.result);
                        this.form.photo = reader.result;
                    };
                    reader.readAsDataURL(file);
                } else {
                    swal({
                        type: 'error',
                        title: 'Oops...',
                        text : 'You are uploading a large file...',
                    })
                }
                // console.log(e);
            }
        },
        created() {
            // loadProfile();
            axios.get('api/profile')
                .then(({ data }) => { this.form.fill(data); });
        }
    }
</script>
