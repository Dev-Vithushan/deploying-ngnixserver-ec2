
# Deploying a Static Website Using an S3 Bucket

## 1. Create an S3 Bucket
1. **Open AWS Management Console** and navigate to **S3**.
2. Click **Create bucket**:
   - **Bucket Name**: Enter a globally unique name (e.g., `my-static-website`).
   - **Region**: Choose the desired AWS region.
3. Uncheck **Block all public access** (required for static websites).
4. Acknowledge the warning about public access and click **Create bucket**.

---

## 2. Upload Website Files
1. Open the newly created bucket.
2. Click **Upload**.
3. Add your HTML, CSS, JavaScript, and other files.
4. Ensure the **index.html** file is uploaded (this will be the default page).

---

## 3. Configure Bucket for Static Website Hosting
1. Navigate to the **Properties** tab of the bucket.
2. Scroll down to **Static website hosting** and click **Edit**.
3. Enable **Static website hosting**.
4. Enter:
   - **Index document**: `index.html`
   - (Optional) **Error document**: `error.html`
5. Save changes.

---

## 4. Update Bucket Policy for Public Access
1. Go to the **Permissions** tab.
2. Under **Bucket Policy**, click **Edit** and paste the following policy, replacing `BUCKET_NAME` with your bucket name:

   ```json
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Principal": "*",
               "Action": "s3:GetObject",
               "Resource": "arn:aws:s3:::BUCKET_NAME/*"
           }
       ]
   }
   ```

3. Click **Save changes**.

---

## 5. Test the Static Website
1. In the **Properties** tab, copy the **Static website hosting endpoint**.
2. Paste the URL into a browser to view your website.

---

## 6. Optional: Use a Custom Domain
If you want to serve the website via a custom domain:
1. **Set up a domain** in Route 53 or another DNS provider.
2. Add an **Alias Record (CNAME)** in Route 53 pointing to your S3 website endpoint.

---

Let me know if you need further assistance!
