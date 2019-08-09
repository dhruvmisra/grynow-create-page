<template>
  <div id="app">
    <div class="container">
      <h1 class="text-center">Create new Blog page</h1>
      <hr />

      <h2>File</h2>
      <div class="form-group col-6">
        <label for="fileName">File Name</label>
        <input
          type="text"
          class="form-control"
          id="fileName"
          placeholder="File Name (URL)"
          v-model="fileName"
        />
      </div>

      <h2>Head section</h2>
      <form>
        <div class="form-group col-6">
          <label for="title">Title tag</label>
          <input
            type="text"
            class="form-control"
            id="title"
            placeholder="Title"
            v-model="metaTitle"
          />
        </div>
        <div class="form-group col-8">
          <label for="metaDescription">Meta description</label>
          <textarea
            class="form-control"
            name="metaDescription"
            id="metaDescription"
            cols="30"
            rows="5"
            placeholder="Description"
            v-model="metaDescription"
          />
        </div>
      </form>

      <h2>Body section</h2>
      <form>
        <div class="row m-0">
          <div class="form-group col-8">
            <label for="blogTitle">Blog Title</label>
            <input
              type="text"
              class="form-control"
              id="blogTitle"
              placeholder="Blog Title (h1)"
              v-model="blogTitle"
            />
          </div>
          <div class="form-group col-4">
            <label for="author">Author</label>
            <input
              type="text"
              class="form-control"
              id="author"
              placeholder="Author"
              v-model="author"
            />
          </div>
          <div class="form-group col-12">
            <label for="author">Main Image:</label>
            <input
              type="file"
              accept="image/*"
              class="col-10 mx-1"
              :id="blogMainImage.imgId"
              placeholder="Content"
              @change="bindImage($event, blogMainImage)"
            />
          </div>
        </div>

        <div class="form-group col-12 mx-auto">
          <label for="blogContent">Blog Content</label>
          <div id="blogContent">
            <div class="form-group" v-for="item in blogContent" :key="item.id">
              <h5>{{ item.tag }}</h5>

              <div class="row m-0">
                <input
                  type="file"
                  accept="image/*"
                  class="col-10 mx-1"
                  :id="item.imgId"
                  placeholder="Content"
                  v-if="item.tag == 'img'"
                  @change="bindImage($event, item)"
                />

                <div class="col-10" v-else-if="item.tag == 'ul' || item.tag == 'ol'">
                  <ul v-if="item.tag == 'ul'">
                    <li v-for="(li, i) in item.li" :key="i">
                      <div v-for="input in li.inputs" :key="input.id">
                        <h6>{{ input.tag }}</h6>
                        <div class="row m-0">
                          <input
                            type="text"
                            class="form-control col-10 mx-1"
                            placeholder="Content"
                            v-model="input.text"
                          />
                          <button
                            type="button"
                            @click="deleteInput(input.id, li.inputs)"
                            class="btn btn-outline-danger col-1 mx-1"
                          >&#10005;</button>
                        </div>
                      </div>
                      <div class="ml-2">
                        <button
                          type="button"
                          @click="addItem('p', li.inputs)"
                          class="btn btn-outline-info m-1"
                        >p</button>
                        <button
                          type="button"
                          @click="addItem('h3', li.inputs)"
                          class="btn btn-outline-info m-1"
                        >h3</button>
                        <button
                          type="button"
                          @click="deleteListItem(i, item.li)"
                          class="btn btn-outline-danger m-1"
                        >&#10005;</button>
                      </div>
                    </li>
                    <button
                      type="button"
                      @click="addListItem(item.li)"
                      class="btn btn-outline-warning m-1"
                    >Add List Item</button>
                  </ul>
                  <ol v-else>
                    <li v-for="(li, i) in item.li" :key="i">
                      <div v-for="input in li.inputs" :key="input.id">
                        <h6>{{ input.tag }}</h6>
                        <div class="row m-0">
                          <input
                            type="text"
                            class="form-control col-10 mx-1"
                            placeholder="Content"
                            v-model="input.text"
                          />
                          <button
                            type="button"
                            @click="deleteInput(input.id, li.inputs)"
                            class="btn btn-outline-danger col-1 mx-1"
                          >&#10005;</button>
                        </div>
                      </div>
                      <div class="ml-2">
                        <button
                          type="button"
                          @click="addItem('p', li.inputs)"
                          class="btn btn-outline-info m-1"
                        >p</button>
                        <button
                          type="button"
                          @click="addItem('h3', li.inputs)"
                          class="btn btn-outline-info m-1"
                        >h3</button>
                        <button
                          type="button"
                          @click="deleteListItem(i, item.li)"
                          class="btn btn-outline-danger m-1"
                        >&#10005;</button>
                      </div>
                    </li>
                    <button
                      type="button"
                      @click="addListItem(item.li)"
                      class="btn btn-outline-warning m-1"
                    >Add List Item</button>
                  </ol>
                </div>

                <textarea
                  name="para"
                  cols="30"
                  rows="5"
                  class="form-control col-9 mx-1"
                  placeholder="Content"
                  v-else-if="item.tag == 'p'"
                  v-model="item.text"
                ></textarea>

                <div v-else-if="item.tag == 'a'" class="col-10 row">
                  <input
                    type="text"
                    class="form-control col-12 mx-1"
                    placeholder="Content"
                    v-model="item.text"
                  />
                  <input
                    type="text"
                    class="form-control col-8 m-1"
                    placeholder="Link"
                    v-model="item.link"
                  />
                  <div class="custom-control custom-checkbox my-auto">
                    <input
                      type="checkbox"
                      class="custom-control-input"
                      id="nofollow"
                      v-model="item.nofollow"
                    />
                    <label class="custom-control-label" for="nofollow">nofollow</label>
                  </div>
                </div>

                <input
                  type="text"
                  class="form-control col-10 mx-1"
                  placeholder="Content"
                  v-else
                  v-model="item.text"
                />

                <button
                  type="button"
                  @click="deleteInput(item.id, blogContent)"
                  class="btn btn-outline-danger col-1 mx-1 ml-auto"
                >&#10005;</button>
              </div>
            </div>

            <button
              type="button"
              class="btn btn-outline-primary m-1"
              @click="addTextInput('p', blogContent)"
            >p</button>
            <button
              type="button"
              class="btn btn-outline-primary m-1"
              @click="addLink(blogContent)"
            >a</button>
            <button
              type="button"
              class="btn btn-outline-primary m-1"
              @click="addTextInput('h2', blogContent)"
            >h2</button>
            <button
              type="button"
              class="btn btn-outline-primary m-1"
              @click="addList('ul', blogContent)"
            >ul</button>
            <button
              type="button"
              class="btn btn-outline-primary m-1"
              @click="addList('ol', blogContent)"
            >ol</button>
            <button type="button" class="btn btn-outline-primary m-1" @click="addImage()">img</button>
          </div>
        </div>
      </form>

      <!-- <p>{{blogContent}}</p> -->

      <button class="d-block btn btn-danger btn-lg px-5 mx-auto my-5" @click="createPage">Create</button>

      <section v-if="created">
        <div class="container">
          <h6>Follow these steps to add the blog page: </h6>
          <ol>
            <li>Login to Cpanel by going to <a href="http://www.grynow.in/cpanel">http://www.grynow.in/cpanel</a>. </li>
            <li>Open File Manager and open the 'public_html' directory.</li>
            <li>Click the 'Upload' button and upload the zip file just downloaded. Close the upload page when completed.</li>
            <li>Refresh the public_html directory to find the upload.zip file. Right click on the file and 
              select 'Extract'. Click on 'Extract File(s)' button.</li>
            <li>
              Open 'blogs.html' and copy & paste the following code inside the blog list: 
              <div class="col-sm-12 col-md-8 rounded-lg bg-secondary p-5 m-2">
                <p class="text-white">
                  {{ blogsCode }}
                </p>
              </div>
            </li>
          </ol>
        </div>
      </section>
 
    </div>
  </div>
</template>

<script>
import JSZip from 'jszip';
import FileSaver from 'file-saver';

export default {
  name: "app",
  data() {
    return {
      created: false,
      fileName: "",
      metaTitle: "",
      metaDescription: "",
      blogTitle: "",
      author: "",
      blogContent: [],
      blogMainImage: {
        id: 0,
        file: null,
        tag: "img",
        imgId: 1
      },
      blogImagesId: 2,
      blogInputs: 1,
      pageContent: "",
      blogsCode: '',
      zip: null
    };
  },
  computed: {
    fileNameWithExtention() {
      return this.fileName + ".html";
    },
    url() {
      return "http://www.grynow.in/" + this.fileNameWithExtention;
    }
  },
  methods: {
    addTextInput(tag, array) {
      if(tag == 'a') {
        array.push({
          id: this.blogInputs,
          text: "",
          link: '',
          tag: tag
        });
      } else {
        array.push({
          id: this.blogInputs,
          text: "",
          tag: tag
        });
      }
      this.blogInputs++;
    },
    addLink(array) {
      array.push({
        id: this.blogInputs,
        text: "",
        nofollow: true,
        tag: 'a'
      });
      this.blogInputs++;
    },
    addList(tag, array) {
      array.push({
        id: this.blogInputs,
        li: [
          {
            inputs: []
          }
        ],
        tag: tag
      });
      this.blogInputs++;
    },
    addListItem(li) {
      li.push({
        inputs: []
      });
    },
    deleteListItem(index, array) {
      array.splice(index, 1);
    },
    addItem(tag, inputs) {
      inputs.push({
        id: this.blogInputs,
        text: "",
        tag: tag
      });
      this.blogInputs++;
    },
    addImage() {
      this.blogContent.push({
        id: this.blogInputs,
        file: null,
        tag: "img",
        imgId: this.blogImagesId
      });
      this.blogImagesId++;
      this.blogInputs++;
    },
    bindImage(event, item) {
      item.file = event.target.files[0];

      // let type = file.type;
      // let imgName = file.name;
      // let blob = new Blob([file], {type: type});
      // console.log(blob);
      
      // let objectURL = window.URL.createObjectURL(blob);
      // console.log(objectURL);

      // let link = document.createElement('a');
      // link.href = objectURL;
      // link.download = imgName;
      // document.body.appendChild(link);
      // link.click();
      // link.remove();
    },
    deleteInput(id, array) {
      const index = array.findIndex(item => {
        return item.id == id;
      });
      array.splice(index, 1);
    },
    createPage() {
      this.zip = new JSZip();

      this.pageContent = `<!DOCTYPE html>
<html lang="en">
<head>
  <!-- Global site tag (gtag.js) - Google Analytics -->
  <scr` + `ipt async src="https://www.googletagmanager.com/gtag/js?id=UA-143978554-1"></scr` + `ipt>
  <scr` + `ipt>
      window.dataLayer = window.dataLayer || [];
      function gtag(){dataLayer.push(arguments);}
      gtag('js', new Date());
      
      gtag('config', 'UA-143978554-1');
      gtag('config', 'AW-721210182');
  </scr` + `ipt>

  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  
  <title>${this.metaTitle}</title>
	<link rel="icon" type="image/ico" href="../assets/logo.png" />
  <meta name="description" content="${this.metaDescription}">

  <!-- Canonical tag -->
  <link rel="canonical" href="http://www.grynow.in/blog/${this.fileNameWithExtention}" />
  
  <!-- Bootstrap -->
	<link rel="stylesheet" type="text/css" href="../css/bootstrap.min.css">
	<!-- Custom Bootstrap -->
	<link rel="stylesheet" type="text/css" href="../css/custom-bootstrap.css">
	<!-- Custom CSS -->
	<link rel="stylesheet" type="text/css" href="../css/styles.css">
  <link rel="stylesheet" type="text/css" href="../css/styles-blog.css">

  <!--Start of Tawk.to Script-->
  <scr` + `ipt type="text/javascript">
      var Tawk_API=Tawk_API||{}, Tawk_LoadStart=new Date();
      (function(){
      var s1=document.createElement("script"),s0=document.getElementsByTagName("scr` + `ipt")[0];
      s1.async=true;
      s1.src='https://embed.tawk.to/5cfd335b267b2e5785318955/default';
      s1.charset='UTF-8';
      s1.setAttribute('crossorigin','*');
      s0.parentNode.insertBefore(s1,s0);
      })();
  </scr` + `ipt>
  <!--End of Tawk.to Script-->	
  
  <!-- Facebook Pixel Code -->
  <scr` + `ipt>
    !function(f,b,e,v,n,t,s)
    {if(f.fbq)return;n=f.fbq=function(){n.callMethod?
    n.callMethod.apply(n,arguments):n.queue.push(arguments)};
    if(!f._fbq)f._fbq=n;n.push=n;n.loaded=!0;n.version='2.0';
    n.queue=[];t=b.createElement(e);t.async=!0;
    t.src=v;s=b.getElementsByTagName(e)[0];
    s.parentNode.insertBefore(t,s)}(window, document,'script',
    'https://connect.facebook.net/en_US/fbevents.js');
    fbq('init', '2357150577939123');
    fbq('track', 'PageView');
  </scr` + `ipt>
  <noscr` + `ipt><img height="1" width="1" style="display:none"
    src="https://www.facebook.com/tr?id=2357150577939123&ev=PageView&noscript=1"
  /></noscr` + `ipt>
  <!-- End Facebook Pixel Code -->
</head>
<body>

  <!-- Navbar -->
	<nav class="navbar navbar-expand-lg navbar-light bg-transparent">
		<div class="container">
			<a class="navbar-brand" href="../" style="padding-top: 0">
				<img src="../assets/logo_with_title.png" alt="GryNow logo" class="brand-icon">
				<!-- <h2 class="brand-name align-middle">Gry<span>Now</span></h2> -->
			</a>
			<button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNavAltMarkup"
				aria-controls="navbarNavAltMarkup" aria-expanded="false" aria-label="Toggle navigation">
				<span class="navbar-toggler-icon"></span>
			</button>
      <div class="collapse navbar-collapse" id="navbarNavAltMarkup">
        <div class="navbar-nav">
          <a class="nav-item nav-link" href="../social-media-influencer-platform-and-market-place.html">Creators <span class="sr-only">(current)</span></a>
          <a class="nav-item nav-link" href="../brand-marketing-management-agency.html">Brands & Agencies</a>
          <a class="nav-item nav-link" href="../case-studies.html">Case Studies</a>
          <a class="nav-item nav-link" href="../blogs.html">Blogs</a>
          <a class="nav-item nav-link" href="../influencer-management-agency-india.html">About Us</a>
        </div>
        <a class="btn btn-outline-primary rounded-pill ml-auto"
          href="../contact.php"
          role="button"
          >Contact Us</a>
      </div>
		</div>
  </nav>
  
  <div class="blog row">

    <div class="sticky-container col-sm-12 col-md-1">
      <div id="share-blog" class="text-center">
        <p class="">Share blog</p>
        <!-- Facebook -->
        <a href="http://www.facebook.com/sharer.php?u=${this.url}" target="_blank">
          <img src="https://simplesharebuttons.com/images/somacro/facebook.png" alt="Facebook" />
        </a>
        <!-- LinkedIn -->
        <a href="http://www.linkedin.com/shareArticle?mini=true&amp;url=${this.url}" target="_blank">
          <img src="https://simplesharebuttons.com/images/somacro/linkedin.png" alt="LinkedIn" />
        </a>
        <!-- Twitter -->
        <a href="https://twitter.com/share?url=${this.url}&amp;text=Grynow%20Influncer%20Marketing%20Blog&amp;hashtags=grynow" target="_blank">
          <img src="https://simplesharebuttons.com/images/somacro/twitter.png" alt="Twitter" />
        </a>
        <!-- Email -->
        <a href="mailto:?Subject=Simple Share Buttons&amp;Body=I%20saw%20this%20and%20thought%20of%20you!%20 ${this.url}">
          <img src="https://simplesharebuttons.com/images/somacro/email.png" alt="Email" />
        </a>     
      </div>
    </div>

    <div class="container">

      <h1 class="text-center">${this.blogTitle}</h1>

      <div class="blog-image">
        <img src="../assets/blogs/${this.fileName}1.jpg" alt="Influencer-marketing" class="w-100">
      </div>

      <div class="blog-content col-md-8 mx-auto">
`;

      for(let i=0; i<this.blogContent.length; i++) {
        let item = this.blogContent[i];
        switch(item.tag) {
          case 'p': {
            this.pageContent += `
              <p>${item.text}</p>`;
            break;
          }
          case 'h2': {
            this.pageContent += `
              <h2>${item.text}</h2>`;
            break;
          }
          case 'a': {
            this.pageContent += `
              <a `;
            if(item.nofollow) {
              this.pageContent += `rel="nofollow" `;
            }
            this.pageContent += `href="${item.link}">${item.text}</a>`;
            break;
          }
          case 'ul': {
            this.pageContent += `
              <ul>`;
                for(let j=0; j<item.li.length; j++) {
                  let li = item.li[j];
                  this.pageContent += `
                    <li>`;
                  for(let k=0; k<li.inputs.length; k++) {
                    let input = li.inputs[k];
                    switch(input.tag) {
                      case 'p': {
                        this.pageContent += `
                          <p>${input.text}</p>`;
                        break;
                      }
                      case 'h3': {
                        this.pageContent += `
                          <h3>${input.text}</h3>`;
                        break;
                      }
                    }
                  }
                  this.pageContent += `
                    </li>`;
                }
            this.pageContent += `
              </ul>`;
              break;
          }
          case 'ol': {
            this.pageContent += `
              <ol>`;
                for(let li in item.li) {
                  this.pageContent += `
                    <li>`;
                  for(let input in li.inputs) {
                    switch(input.tag) {
                      case 'p': {
                        this.pageContent += `
                          <p>${item.text}</p>`;
                        break;
                      }
                      case 'h3': {
                        this.pageContent += `
                          <h3>${item.text}</h3>`;
                        break;
                      }
                    }
                  }
                  this.pageContent += `
                    </li>`;
                }
            this.pageContent += `
              </ol>`;
              break;
          }
          case 'img': {
            this.pageContent += `
            <img src="../assets/blogs/${this.fileName}${item.imgId}.jpg" alt="${this.fileName}-${item.imgId}" class="w-100 mt-3">`;
          }
        }
      }
      //Author
      this.pageContent += `
        <span class="author">- ${this.author}</span>`;

      this.pageContent += `
  </div>

      <div class="my-5 py-5 text-center">
        <h5>Similar blogs:</h5>
        <ul style="list-style-type: none">
          <li><a href="http://www.grynow.in/blog/influencer-marketing-in-india.html">Influencer Marketing in India: How Brands can grow with it</a></li>
        </ul>
      </div>

    </div>
  </div>

  <!-- footer -->
	<footer class="footer">
		<div class="container">
			<div class="social-row">
				<p class="text-white text-center">Follow Us</p>
				<div class="d-flex justify-content-center">
					<a rel="nofollow" href="https://www.youtube.com/channel/UChCwo_Lrp1pljzDhevETeGw" target="_blank">
						<img src="../assets/social icons/youtube.png" alt="" class="social">
					</a>
					<a rel="nofollow" href="https://www.instagram.com/gry_now" target="_blank">
						<img src="../assets/social icons/instagram.png" alt="" class="social">
					</a>
					<a rel="nofollow" href="https://facebook.com/grynow" target="_blank">
						<img src="../assets/social icons/facebook.png" alt="" class="social">
					</a>
					<a rel="nofollow" href="https://www.linkedin.com/company/gry/" target="_blank">
						<img src="../assets/social icons/linkedin.png" alt="" class="social">
					</a>
					<a rel="nofollow" href="https://twitter.com/Grynowagency" target="_blank">
						<img src="../assets/social icons/twitter.png" alt="" class="social">
					</a>
				</div>
			</div>
			<br>
			<hr style="background-color: white">
			<br>
			<div class="col-md-12 col-lg-4 text-white row">
				<img src="../assets/logo_with_title.png" 
					alt="GryNow logo" 
					class="brand-icon">
				</div>
			<br>
			<div class="row text-white">
				<div class="col-md-12 col-lg-4">
					<ul style="list-style-type: none;">
						<li class="list-title">About</li>
						<li class="list-element">GryNow is the leading platform for brands, looking for influencers/creators, 
							to market their products.</li>
					</ul>
				</div>
				<div class="col-xs-3 col-lg-4">
					<ul style="list-style-type: none;">
						<li class="list-title">Navigate</li>
						<li><a class="list-link" href="../social-media-influencer-platform-and-market-place.html">CREATORS</a></li>
						<li><a class="list-link" href="../brand-marketing-management-agency.html">BRANDS & AGENCIES</a></li>
						<li><a class="list-link" href="../case-studies.html">CASE STUDIES</a></li>
						<li><a class="list-link" href="../blogs.html">BLOGS</a></li>
						<li><a class="list-link" href="../videos-social-media-content-by-grynow.html">VIDEOS</a></li>
						<li><a class="list-link" href="../influencer-management-agency-india.html">ABOUT US</a></li>
					</ul>
				</div>
				<div class="col-xs-3 col-lg-4">
					<ul style="list-style-type: none;">
						<li class="list-title">Contact Us</li>
						<li>820, Golf Course Road</li>
						<li>Sector 42, Gurugram</li>
						<li>Haryana - 122001, India</li>
						<br>
						<li>Email: <a class="list-link" href="mailto:aman@grynow.in">aman@grynow.in</a></li>
						<li>Contact: <a class="list-link" href="tel:9917486405">+91 9917486405</a></li>
					</ul>
				</div>
			</div>

		</div>
		<div class="copyright">&copy; 2017-2019 GryNow | All Rights Reserved</div>
  </footer>
  
  <!-- Bootstrap -->
	<scr` + `ipt src="https://code.jquery.com/jquery-3.3.1.slim.min.js"
	integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo"
	crossorigin="anonymous"></scr` + `ipt>
	<scr` + `ipt src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.7/umd/popper.min.js"
	integrity="sha384-UO2eT0CpHqdSJQ6hJty5KVphtPhzWj9WO1clHTMGa3JDZwrnQq4sF86dIHNDz0W1"
	crossorigin="anonymous"></scr` + `ipt>
	<scr` + `ipt src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js"
	integrity="sha384-JjSmVgyd0p3pXB1rRibZUAYoIIy6OrQ6VrjIEaFf/nJGzIxFDsf4x0xIM+B07jRM"
	crossorigin="anonymous"></scr` + `ipt>


</body>
</html>`;

      this.blogsCode = `<div class="card blog col-sm-12 col-md-6 col-lg-3">
					<a class="blog-link" href="./blog/${this.fileNameWithExtention}">			
						<div class="blog-image">
							<img src="./assets/blogs/${this.fileName}1.jpg" alt="${this.fileName}" class="w-100">
						</div>
						<div class="card-body">
							<h4 class="card-title">${this.blogTitle}</h4>
							<br>
							<span>${this.author}</span>
						</div>
					</a>
				</div>`;

      this.zip.folder("blog").file(this.fileNameWithExtention, this.pageContent);

      let name;
      for(let i=0; i<this.blogContent.length; i++) {
        if(this.blogContent[i].tag == 'img') {
          console.log('inside loop');
          name = this.fileName + this.blogContent[i].imgId + '.jpg';
          this.zip.folder("assets").folder("blogs").file(name, this.blogContent[i].file);
        }
      }
      name = this.fileName + this.blogMainImage.imgId + '.jpg';
      this.zip.folder("assets").folder("blogs").file(name, this.blogMainImage.file);
      
      this.zip.generateAsync({type: "blob"}).then(function(content) {
        FileSaver.saveAs(content, "upload.zip");
        this.created = true;
      });
    },
    downloadFile() {
      let file = new Blob([this.pageContent], { type: "text/plain" });
      if (window.navigator.msSaveOrOpenBlob)
        // IE10+
        window.navigator.msSaveOrOpenBlob(file, this.fileNameWithExtention);
      else {
        // Others
        let a = document.createElement("a"),
          url = URL.createObjectURL(file);
        a.href = url;
        a.download = this.fileNameWithExtention;
        document.body.appendChild(a);
        a.click();
        setTimeout(function() {
          document.body.removeChild(a);
          window.URL.revokeObjectURL(url);
        }, 0);
      }
    }
  }
};
</script>

<style>
#app {
  background-color: rgb(248, 253, 255);
}
</style>
