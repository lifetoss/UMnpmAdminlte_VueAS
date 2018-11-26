<template>
    <div class="container">
        
        
        <div class="row mt-5">
          <div class="col-12">
            <div class="card">
              <div class="card-header">
                <h3 class="card-title">Users Table</h3>

                <div class="card-tools">
                	<button class="btn btn-success" data-toggle="modal" data-target="#exampleModal">
                	<i class="fas fa-user-plus"></i>
                	Add User</button>
              </div>
              </div>
              <!-- /.card-header -->
              <div class="card-body table-responsive p-0">
                <table class="table table-hover">
                  <tbody><tr>
                    <th>ID</th>
                    <th>User</th>
                    <th>Date</th>
                    <th>Status</th>
                    <th>Reason</th>
                  </tr>
                  <tr v-for="user in users" :key="user.id">
	                  <td>{{user.id}}</td>
	                  <td>{{user.name}}</td>
	                  <td>{{user.email}}</td>
	                  <td>{{user.type}}</td>
	                  <td>{{user.created_at}}</td>
	                  <td>
                        <a href=""><i class="fa fa-edit green"></i></a>/
                        <a href=""><i class="fa fa-trash red"></i></a>
                    </td>
                  </tr>
                </tbody></table>
              </div>
              <!-- /.card-body -->
            </div>
            <!-- /.card -->
          </div>
        </div>

        <!-- Modal -->
        <div class="modal fade" id="exampleModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
        <div class="modal-dialog" role="document">
            <div class="modal-content">
            <div class="modal-header">
                <h5 class="modal-title" id="exampleModalLabel">Modal title</h5>
                <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                <span aria-hidden="true">&times;</span>
                </button>
            </div>
            <form @submit.prevent="createUser" >
                <div class="modal-body">

                      <div class="form-group">
                       <!-- form.name here this is function we call which is define at the bottom-->	
                        <input v-model="form.name" type="text" name="name"
                        placeholder="Enter Your Name"
                          class="form-control" :class="{ 'is-invalid': form.errors.has('name') }">
                        <has-error :form="form" field="name"></has-error>
                      </div>

                      <div class="form-group">
                        <input v-model="form.email" type="email" name="email"
                        placeholder="Enter Your Email Address"
                          class="form-control" :class="{ 'is-invalid': form.errors.has('email') }">
                        <has-error :form="form" field="email"></has-error>
                      </div>

                      <div class="form-group">
                        <textarea v-model="form.bio" name="bio"
                        placeholder="short bio for user (optinal)"
                          class="form-control" :class="{ 'is-invalid': form.errors.has('bio') }"></textarea>
                        <has-error :form="form" field="bio"></has-error>
                      </div>

                      <div class="form-group">
                      <select v-model="form.type" name="type" id="type"
                          class="form-control" :class="{ 'is-invalid': form.errors.has('type') }">
                          <option value="">Select Your Role</option>
                          <option value="admin">Admin</option>
                          <option value="user">Standard User</option>
                          <option value="author">Author</option>
                      </select>							      
                        <has-error :form="form" field="type"></has-error>
                      </div>

                      <div class="form-group">
                        <input v-model="form.password" type="password" name="password" id="password"
                        placeholder="Your Password"
                          class="form-control" :class="{ 'is-invalid': form.errors.has('password') }">
                        <has-error :form="form" field="password"></has-error>
                      </div>

                </div>
                
                <div class="modal-footer">
                  <button type="button" class="btn btn-danger" data-dismiss="modal">Close</button>
                  <button type="submit" class="btn btn-success">Save changes</button>
                </div>
                </form>


            </div>
        </div>
        </div>
        <!-- ./Modal -->

    </div>
</template>

<script>
    export default {

         data() {
            return{
          users :{},	// here {} this is object and [] this is array
          // this new Form is instance from window.Form = Form
          form: new Form({
            	id: '',
            	name: '',
            	email: '',
            	password: '',
            	type: '',
            	bio: '',
            	photo: ''
            })
            } 
        },
        methods:{
          loadUsers(){
        		axios.get("api/user").then(({ data }) => (this.users = data.data));
              },
          createUser(){
            // here is way of inserting data     this.form.post('api/user')
            this.form.post('api/user')
            Fire.$emit('AfterCreate');
          }
              
        },

        created() {
            this.loadUsers();
            Fire.$on('AfterCreate',()=>{
              this.loadUsers();
            })
        }
    }
</script>
