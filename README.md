# GlideLoader

package com.stocksgomad.utils

import android.content.Context
import android.net.Uri
import android.widget.ImageView
import com.bumptech.glide.Glide
import com.bumptech.glide.Priority
import com.bumptech.glide.load.engine.DiskCacheStrategy
import com.bumptech.glide.load.resource.bitmap.CenterCrop
import com.bumptech.glide.load.resource.bitmap.RoundedCorners
import com.bumptech.glide.request.RequestOptions
import com.bumptech.glide.signature.ObjectKey
import com.google.android.material.imageview.ShapeableImageView
import com.stocksgomad.R
import com.stocksgomad.utils.pref.SessionManager
import kotlinx.coroutines.GlobalScope
import kotlinx.coroutines.MainScope
import kotlinx.coroutines.launch
import java.io.File


class GlideLoader(private val context: Context) {


    private val simpleOptions = RequestOptions()
        .fitCenter()
        .placeholder(R.color.instrument_bg)
        .error(R.color.instrument_bg)
        .diskCacheStrategy(DiskCacheStrategy.RESOURCE)

    private val withoutCache = RequestOptions()
        .fitCenter()
        .diskCacheStrategy(DiskCacheStrategy.RESOURCE)

    private val bgOptions = RequestOptions()
        .fitCenter()
        .placeholder(R.drawable.ic_bg_n_50)
        .error(R.drawable.ic_bg_n_50)
        .diskCacheStrategy(DiskCacheStrategy.RESOURCE)


    private val withoutCacheProfile = RequestOptions()
        .fitCenter()
        .diskCacheStrategy(DiskCacheStrategy.RESOURCE)
        .signature(ObjectKey(SessionManager(context).getDataByKey("imgTimeStamp")))

    private val circleOptions = RequestOptions()
        .circleCrop()
        // .error(R.drawable.ic_placeholder)
        // .placeholder(R.drawable.ic_placeholder)
//        .skipMemoryCache(false)
        .diskCacheStrategy(DiskCacheStrategy.RESOURCE)

    private val roundedCorners = RequestOptions()
        .placeholder(R.color.colorGray)
        .error(R.color.colorGray)
        .transforms(CenterCrop(), RoundedCorners(30))
        .diskCacheStrategy(DiskCacheStrategy.AUTOMATIC)

    private val circleOptionsError = RequestOptions()
        .circleCrop()
        .placeholder(R.color.colorGray)
        .error(R.color.colorGray)
        .diskCacheStrategy(DiskCacheStrategy.ALL)

    private val circleOptionsErrorNoPlaceHolder = RequestOptions()
        .circleCrop()
        .error(R.color.colorGray)
        .diskCacheStrategy(DiskCacheStrategy.ALL)


    /**
     *@param url - URL for downloading the image
     * @param view - image view in which the downloaded image will be displayed without any format
     */
    fun loadImageSimple(url: String, view: ImageView) {
        Glide.with(context)
            .load(url)
            .apply(simpleOptions)
            .into(view)
    }

    /**
     *@param url - URL for downloading the image
     * @param view - image view in which the downloaded image will be displayed without any format
     */
    fun loadImageProfile(url: String, view: ImageView) {
        Glide.with(context)
            .load(url)
            .apply(withoutCacheProfile)
            .into(view)
    }

    /**
     *@param url - URL for downloading the image
     * @param view - image view in which the downloaded image will be displayed without any format
     */
    fun loadImageNoCache(url: String, view: ShapeableImageView) {
        Glide.with(context)
            .load(url)
            .apply(withoutCache)
            .into(view)
    }

    /**
     *@param url - URL for downloading the image
     * @param view - image view in which the downloaded image will be displayed without any format
     */
    fun loadImageNoCacheUserImage(url: String, view: ShapeableImageView, timeStamp : String) {
        Glide.with(context)
            .load(url)
            .apply(withoutCacheProfile)
            .into(view)
    }

    /**
     *@param url - URL for downloading the image
     * @param view - image view in which the downloaded image will be displayed without any format
     */
    fun loadImageNoCacheUserImage(url: String, view: ImageView) {
        Glide.with(context)
            .load(url)
            .apply(withoutCacheProfile)
            .into(view)
    }

    /**
     *@param url - URL for downloading the image
     * @param view - image view in which the downloaded image will be displayed without any format
     */
    fun loadImageNoCache(url: Uri, view: ShapeableImageView) {
        Glide.with(context)
            .load(url)
            .apply(withoutCache)
            .into(view)
    }

    fun loadImageBG(url: String, view: ImageView) {
        Glide.with(context)
            .load(url)
            .apply(bgOptions)
            .into(view)
    }

    /**
     * @param uri - URI of the image to be loaded
     * @param view - image view in which the downloaded image will be displayed without any format
     */
    fun loadImageSimple(uri: Uri?, view: ImageView) {
        Glide.with(context)
            .load(uri)
            .apply(simpleOptions)
            .into(view)
    }

    /**
     * @param uri - URI of the image to be loaded
     * @param view - image view in which the downloaded image will be displayed without any format
     */
    fun loadImageSimpleCircle(uri: Uri?, view: ImageView) {
        Glide.with(context)
            .load(uri)
            .apply(circleOptions)
            .into(view)
    }

    /**
     *@param url - URL for downloading the image
     * @param view - image view in which the downloaded image will be displayed in a circle
     */
    fun loadImageCircle(url: String?, view: ImageView) {
        Glide.with(context)
            .load(url)
            .apply(circleOptions)
            .into(view)
    }

    fun loadImageCircleWithErrorImg(url: String?, view: ImageView) {
        Glide.with(context)
            .load(url)
            .apply(circleOptionsError)
            .into(view)
    }

    /**
     *@param file - path of file that is to be loaded
     * @param view - image view in which the downloaded image will be displayed in a circle
     */

    fun loadImageFromFile(file: File, view: ImageView) {
        Glide.with(context)
            .load(file)
            .apply(simpleOptions)
            .into(view)
    }

    fun loadRoundCornerImageFromFile(file: File, view: ImageView) {
        Glide.with(context)
            .load(file)
            .apply(roundedCorners)
            .into(view)
    }

    fun loadRoundCornerImageUrl(url: String?, view: ImageView) {
        Glide.with(context)
            .load(url)
            .apply(roundedCorners)
            .into(view)
    }

    fun loadCircleImageFromFile(file: File, view: ImageView) {
        Glide.with(context)
            .load(file)
            .apply(circleOptions)
            .into(view)
    }

    fun removeGlideCache() {
            Glide.get(context).clearMemory()
        GlobalScope.launch {
            Glide.get(context).clearDiskCache()
        }
    }

    /**
     * Gets the GlideLoader singleton
     *
     * @return the GlideLoader singleton
     */
    companion object {
        private var loader: GlideLoader? = null
        fun getInstance(context: Context): GlideLoader {
            loader = GlideLoader(context)
            /*if (loader == null) {
                loader = GlideLoader(context)
            }*/
            return loader as GlideLoader
        }
    }

    /*fun loadImageForZoom(url: String, view: SubsamplingScaleImageView) {
        val requestBuilder = Glide.with(context).asBitmap()
        requestBuilder.load(url)
                .apply(simpleOptions)
                .into(object : SimpleTarget<Bitmap>() {
                    override fun onResourceReady(resource: Bitmap, transition: Transition<in Bitmap>?) {
                        view.setImage(ImageSource.bitmap(resource))
                    }
                })
    }*/

}
